# Find Domain Computers
---

#### 🤓 RTFM

[NetBIOS](https://techterms.com/definition/netbios#google_vignette) Name Service (NTB-NS) is a protocol that translates NetBIOS names to IP addresses by requesting Active Directory Domain Services (AD-DS).

Fortunately for us, it is possible to reverse this process, - *i.e. to recover NetBIOS names from IP addresses*.

#### 🛠️ Do it!

# [NsLookup](#tab/nslookup)

Considering `computers_ip.txt` contains a list of IP, one per line and `$dns_server` should correspond in most cases to the domain controller IP.

```bash
for ip in $(cat computers_ip.txt); do nslookup $ip $dns_server; done
```

# [NbtScan](#tab/nbtscan)

```bash
nbtscan -r $Subnet/$Mask
```

# [NmbLookup](#tab/nmblookup)

Considering `computers_ip.txt` contains a list of IP, one per line.

```bash
for ip in $(cat computers_ip.txt); do nmblookup $ip; done
```

# [NetExec](#tab/netexec)

A more straight forward method consists in scanning IP ranges on SMB port using [NetExec](https://github.com/Pennyw0rth/NetExec).

```bash
nxc smb $Subnet/$Mask
```

---

#### 🏆 What's next?

It's now time to gain an [initial access](/docs/active-directory/methodology.html#-initial-access) to the domain! It can also be accomplished by gaining an [initial access](/docs/windows/methodology.html#-initial-access) to a Windows domain joined computer.

But first, I would suggest you to attempt [user spraying](/docs/active-directory/recon/user-spraying.html), you will surely need it later.