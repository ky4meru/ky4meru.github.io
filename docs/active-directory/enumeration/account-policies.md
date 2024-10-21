# 🔑 Account Policies
---

#### 🤓 RTFM

In Active Directory, it exists 3 types of Account Policies. They are configured by GPO then linked to the root of the domain.
- Password Policy
- Account Lockout Policy
- Kerberos Policy

The Password Policy allows to configured following settings.

| Setting | Description | Default |
| ------- | ----------- | ---------|
| `Enforce password history` | How many times you must change your password before being authorized to reuse an old password | 24 |
| `Maximum password age`  | How long (in days) a password can be used before it needs to be changed | 42 |
| `Minimum password age`  | How long (in days) a password can be used before it can be changed | 1 |
| `Minimum password length` | How many characters a password must have | 7 |
| `Password must meet complexity requirements` | Password must contain uppercase characters, lowercase characters, digits and non-alphabetic characters | Enabled |
| `Store passwords using reversible encryption` | Passwords are stored using reversible encryption, almost the same as plaintext | Disabled |

The Account Lockout Policy allows to configured following settings. **By default, Active Directory has no Account Lockout Policy**.

| Setting | Description | Default |
| ------- | ----------- | ---------|
| `Account lockout duration` | How long (in minutes) an account is locked out before it automatically unlocks | Not defined |
| `Account lockout threshold`  | How many failed logon attempts that caused a user account to be locked out | Not defined |
| `Reset account lockout counter after`  | The time (in minutes) to reset the failed logon attempts counter | Not defined |

#### 📝 Prerequisites

- Authenticated access to the domain.

#### 🛠️ Do it!

Depending on where you are on the way to domain administrator, there are plenty of ways to get the domain password policy and account lockout policy.

# [NetExec](#tab/netexec)

Using [NetExec](https://github.com/Pennyw0rth/NetExec).

```bash
nxc smb $DomainControllerIP -d $Domain -u $Username -p $Password --pass-pol
```
# [ActiveDirectory PowerShell module](#tab/powershell)

From a domain joined computer with [ActiveDirectory](https://learn.microsoft.com/en-us/powershell/module/activedirectory/?view=windowsserver2022-ps) PowerShell module.

```powershell
Get-ADDefaultDomainPasswordPolicy
```

# [Net](#tab/net)

From a domain joined computer with [Net](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/net-commands-on-operating-systems) command.

```bash
net accounts /domain
```

# [Group Policy Management](#tab/gpomanagement)

If you have an access to the Group Policy Management Console, follow these steps.

1. Select `Domains`
2. Select the target domain
3. Select `Group Policy Objects`
4. Right click on `Default Domain Policy`

It will open the Group Policy Management Editor. You can find the password policy at `Computer Configuration\Policies\Windows Settings\Security Settings\Account Policies\Password Policy`.

---

> [!WARNING]
> **It is possible that several password policies exist.**
>
> Indeed, the company could implement another GPO and link it to specific Organizational Units (OU). Another way to do it is to [create fine grained password policies](https://activedirectorypro.com/create-fine-grained-password-policies/) you can assign to specific users and groups.
>
> Knowing that, it means that the **methods above get the password policy enforced for the currently logged on user**. For instance, `Account lockout threshold` could be set to `None` for you, but would be enforced for all domain privileged users.
>
> To get all password policies and there associated groups, use [ldapsearch-ad](https://github.com/yaap7/ldapsearch-ad) python script.
>
> ```bash
> ldapsearch-ad.py -d $domain -u $username -p $password -l $domain_controller_ip -t pass-pols
> ```

#### 🏆 What's next?

If `Account lockout threshold` is set to `None`, you can perform [brute-forcing](/docs/active-directory/initial-access/brute-force.html) or [password spraying](/docs/active-directory/initial-access/password-spraying.html) without risking to lockout accounts.