# 🎯 Active Directory
---

#### 🔍 Recon

Before starting to play with following techniques, it is a good idea to recover information about the [Active Directory](https://techterms.com/definition/active_directory) domain you are going to target as an unauthenticated user.

- [Find Domain Name](/docs/active-directory/recon/domain-name.html)
- [Find Domain Computers](/docs/active-directory/recon/domain-computers.html)
- [Find Domain Controllers](/docs/active-directory/recon/domain-controllers.html)
- [Link-local Multicast Name Resolution](/docs/active-directory/recon/llmnr.html)
- [User Spraying](/docs/active-directory/recon/user-spraying.html)

#### 🔑 Initial access

The very first thing you want to achieve to compromise a domain is to get an initial authenticated access, even low privileged. To do so, following techniques can be leveraged to gain a domain user or computer account.

- [Brute-force](/docs/active-directory/initial-access/brute-force.html)
- [Email Password Spraying](/docs/active-directory/initial-access/email-password-spraying.html)
- [NTLM Relay]()
- [Password Spraying](/docs/active-directory/initial-access/password-spraying.html)
- [PetitPotam [CVE-2022-26925]](/docs/active-directory/initial-access/petit-potam.html)
- [Pre-Windows 2000 Computers](/docs/active-directory/initial-access/pre-windows-2000-computers.html)
- [Printer Spoofing](/docs/active-directory/initial-access/printer-spoofing.html)
- [Zerologon [CVE-2020-1472]](/docs/active-directory/initial-access/zerologon.html)

> [!NOTE]
> You also should consider [Windows initial access](/windows/#without-an-account) techniques on Windows domain joined computers.

#### 🔓 Enumeration

Now you are authenticated on the domain, you can enumerate a lot of useful information that will help you for the next steps. Note that Active Directory enumeration is not a vulnerability by itself. Actually, it is more like a feature because Active Directory is built this way. Once you are authenticated on a domain, you can list all Active Directory objects and their relations. It is a very useful way to discover how to attack the domain by better understanding how it is organized.

- [Account Policies](/docs/active-directory/enumeration/account-policy.html)
- [AppLocker](/docs/active-directory/enumeration/app-locker.html)
- [Automated Enumeration](/docs/active-directory/enumeration/automated-tools.html)
- [Delegations](/docs/active-directory/enumeration/delegations.html)
- [Group Policy Objects](/docs/active-directory/enumeration/gpo.html)
- [Local Administrator Password Solution](/docs/active-directory/enumeration/laps.html)
- [System Center Configuration Manager](/docs/active-directory/enumeration/sccm.html)
- [Shares](/docs/active-directory/enumeration/shares.html)
- [SYSVOL](/docs/active-directory/enumeration/sysvol.html)
- [Trust Relationships](/docs/active-directory/enumeration/trust-relationships.html)
- [Users](/docs/active-directory/enumeration/users.html)

#### 💣 Privilege escalation

Once you get either a low privileged domain account or an access to a domain joined computer, you can perform following attacks to escalate your privileges locally on domain computers or on the domain itself.

- [AppLocker Bypass](/ad/applocker/)
- [AS-REP Roasting](/ad/asreproasting/)
- [Certificate Services](/ad/certs/)
- [Constrained Delegation](/ad/constrained/)
- [DCSync Attack](/ad/dcsync/)
- [Forged Certificates](/ad/forgedcert/)
- [GPO Abuse](/ad/gpo/)
- [Kerberoasting](/ad/kerberoasting/)
- [LAPS Abuse](/ad/laps/)
- [NoPac [CVE-2021-42278] & [CVE-2021-42287]](/ad/nopac/)
- [PetitPotam [CVE-2022-26925]](/ad/petitpotam/)
- [Pre Windows 2000 Compatibility Abuse](/ad/compatibility/)
- [PrintNightmare [CVE-2021-1675]](/ad/printnightmare/)
- [Role-Based Constrained Delegation](/ad/rbconstrained/)
- [SCCM Abuse](/ad/sccm/)
- [S4U2Self Abuse](/ad/s4u2self/)
- [Shadow Credentials](/ad/shadow/)
- [SYSVOL Enumeration](/ad/sysvol/)
- [Unconstrained Delegation](/ad/unconstrained/)

You also should take a look at [Windows Privilege Escalation](/windows/#privilege-escalation) methods.

#### ⚔️ Lateralization

- [Net-NTLMv2 Relay](/ad/netntlmv2relay/)
- [Overpass the Hash](/ad/overpassthehash/)
- [Pass the Hash](/ad/passthehash/)
- [Pass the Ticket](/ad/passtheticket/)
- [Service Name Abuse](/ad/servicename/)
- [Silver Tickets](/ad/silver/)
- [Trust Relationship Abuse](/ad/trust/)

You also should take a look at [Windows Lateralization](/windows/#lateralization) methods.

#### 🚪 Persistence

- [Diamond Tickets](/ad/diamond/)
- [Golden Tickets](/ad/golden/)
- [MachineAccountQuota Abuse](/ad/quota/)

#### 🔗 Useful links

- [ODC Mind Map](https://orange-cyberdefense.github.io/ocd-mindmaps/img/pentest_ad_dark_2023_02.svg) for the Orange Cyberdefense Active Directory mind map.