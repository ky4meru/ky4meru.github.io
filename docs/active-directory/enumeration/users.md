# Users
---

```powershell
# Using Net.exe
net user /domain
net user
net user $username /domain
net group /domain
net localgroup
net group $group [/domain]
net accounts

# Using ActiveDirectory PowerShell module.
Get-ADGroupMember 'domain admins' | select samaccountname
Get-ADUser -Filter {PasswordExpired -eq $True} -Properties PasswordLastSet, PasswordExpired, PasswordNeverExpires | Sort-Object Name
```

```bash
# With NetExec (https://github.com/Pennyw0rth/NetExec).
nxc smb $dc_ip -d $domain -u $username -p $password --users

# From a domain joined Windows computer.
net user /domain

# From a domain joined computer with DomainPasswordSpray PowerShell module (https://github.com/dafthack/DomainPasswordSpray).
Get-DomainUserList -Domain domainname -RemoveDisabled -RemovePotentialLockouts | Out-File -Encoding ascii userlist.txt
```