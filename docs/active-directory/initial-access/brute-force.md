# 🔨 Brute-force
---

> [!CAUTION]
> **Please RTFM carefully.**
>
> This technique can lock a large amount of - *even all* - domain accounts. Be careful to not perform to much attempts on the same usernames or ensure to know the enforced password policy first.

#### 🤓 RTFM

Brute-forcing an Active Directory domain consist in trying a large amount of password for one or multiple usernames until finding legit credentials.

The main protection against brute-force attacks is to enforce password policies on Active Directory, including following options which will be essential to consider for this technique.

1. Reset Account Lockout Counter (minutes).
2. Account Lockout Threshold (number of attempts).

If option `2` is set to `None`, you can perform brute-force without taking the risk to lock accounts.

Otherwise, the account will be locked during the time stated by the option `1`, after have tried to authenticate on the same username the number of attempts stated by the option `2`. Therefore, prefer doing [password spraying](/docs/active-directory/initial-access/password-spraying.html).

#### 📝 Prerequisites

- Network route to a domain joined computer.
- Knowing at least one domain username.
- Knowing the [password policies](/docs/active-directory/enumeration/password-policies.html) enforced on the domain.

#### 🛠️ Do it!

To perform password spraying on a domain, target a domain controller or a domain joined computer if you can't reach a domain controller. You can build yourself the `passwords.txt` wordlist, or you can use [SecLists](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Common-Credentials) ones.

# [NetExec](#tab/netexec)

[NetExec](https://github.com/Pennyw0rth/NetExec) is a cross platform network execution tool, the successor of [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec).

```bash
# Try multiple passwords with the same username.
nxc smb $ComputerIP -d $Domain -u $Username -p passwords.txt

# Try multiple passwords for every username.
nxc smb $ComputerIP -d $Domain -u usernames.txt -p passwords.txt
```

# [Kerbrute](#tab/kerbrute)

[Kerbrute](https://github.com/ropnop/kerbrute) is a cross platform tool to perform Kerberos pre-authentication brute-forcing.

```bash
kerbrute bruteuser --dc $DomainControllerIP -d $Domain passwords.txt $Username
```

---

#### 🏆 What's next?

- If it is your first authenticated access to the domain, let's [enumerate](/docs/active-directory/methodology.html#-enumeration) it!
- [Active Directory privilege escalation](/docs/active-directory/methodology.html#-privilege-escalation) if you gained access to a low privileged domain account.
- [Active Directory persistence](/docs/active-directory/methodology.html#-persistence) if you found a privileged domain account credentials.
- Otherwise, try another method to get an [initial access](/docs/active-directory/methodology.html#-initial-access).