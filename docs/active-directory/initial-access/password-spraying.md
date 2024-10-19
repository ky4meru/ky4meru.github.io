# 🌬️ Password Spraying
---

> [!CAUTION]
> **Please RTFM carefully.**
>
> This technique can lock a large amount of - *even all* - domain accounts. Be careful to not perform to much attempts on the same usernames or ensure to know the enforced password policy first.

#### 🤓 RTFM

For this technique, vulnerability is not really technical. It is simply because of humans that too lazy to put strong and unique passwords.

Password spraying consist in trying a very few passwords, generally default or very simple ones, for a large amount of potential usernames. It is kind of the opposite of [brute-forcing](/docs/active-directory/initial-access/brute-force.html), where you attempt many passwords for one username.

#### 📝 Prerequisites

- Network route to a domain joined computer.
- Knowing multiple domain usernames.
- Knowing the [password policies](/docs/active-directory/enumeration/password-policies.html) enforced on the domain.

#### 🛠️ Do it!

To perform password spraying on a domain, target a domain controller or a domain joined computer if you can't reach a domain controller.

> [!NOTE]
> To build your `usernames.txt` list, several ways are possible:
> - If you already have an initial authenticated access to the domain, you can [retrieve domain users](/docs/active-directory/enumeration/users.html).
> - Otherwise, you can perform [user spraying](/docs/active-directory/recon/user-spraying.html) to validate usernames exist in the domain. 
> - Finally, you can use OSINT tools like [linkedin2username](https://github.com/initstring/linkedin2username).
>
> Usernames often have the same nomenclature like `f.lastname` or `firstname.lastname`. To generate possible usernames based on first name and last name, use [namemash.py](https://gist.github.com/superkojiman/11076951). Anyway, you should first ensure to know the correct terminology before starting password spraying. 

# [NetExec](#tab/netexec)

[NetExec](https://github.com/Pennyw0rth/NetExec) is a cross platform network execution tool, the successor of [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec).

```bash
# Try the same password for every username.
nxc smb $ComputerIP -d $Domain -u usernames.txt -p $Password

# Try multiple passwords for every username.
nxc smb $ComputerIP -d $Domain -u usernames.txt -p passwords.txt

# Try line by line (username1 with password1, username2 with password2, etc.).
nxc smb $ComputerIP -d $Domain -u usernames.txt -p passwords.txt --no-bruteforce
```

# [Kerbrute](#tab/kerbrute)

[Kerbrute](https://github.com/ropnop/kerbrute) is a cross platform tool to perform Kerberos pre-authentication brute-forcing.

```bash
kerberute passwordspray --dc $DomainControllerIP -d $Domain usernames.txt $Password
```

# [DomainPasswordSpray](#tab/domainpasswordspray)

[DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray) is a PowerShell module that can be leveraged from a domain joined computer only. By default, it will automatically retrieve domain users.

```powershell
# Let DomainPasswordSpray automatically retrieving usernames.
Invoke-DomainPasswordSpray -Password $Password

# Provide your own usernames.
Invoke-DomainPasswordSpray -UserList usernames.txt -Password $Password
```

# [DCACLS](#tab/dcacls)

As explained by [ired.team](https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/active-directory-password-spraying#spraying-using-dsacls), it is possible to leverage the builtin executable `dcacls.exe` to perform password spraying.

---

#### 🏆 What's next?

- If it is your first authenticated access to the domain, let's [enumerate](/docs/active-directory/methodology.html#-enumeration) it!
- [Active Directory privilege escalation](/docs/active-directory/methodology.html#-privilege-escalation) if you gained access to a low privileged domain account.
- [Active Directory persistence](/docs/active-directory/methodology.html#-persistence) if you found a privileged domain account credentials.
- Otherwise, try another method to get an [initial access](/docs/active-directory/methodology.html#-initial-access).