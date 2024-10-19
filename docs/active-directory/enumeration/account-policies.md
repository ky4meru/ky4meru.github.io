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

# [NetExec](#tab/netexec)

```bash
nxc smb $DomainControllerIP -d $Domain -u $Username -p $Password --pass-pol
```
# [ActiveDirectory PowerShell module](#tab/powershell)

```powershell
# From a domain joined computer with ActiveDirectory PowerShell module.
Get-ADDefaultDomainPasswordPolicy
```

# [Net](#tab/net)

```bash
# From a domain joined computer with native PowerShell.
net accounts /domain
```

---

#### 🏆 What's next?

If `Account lockout threshold` is set to `None`, you can perform [brute-forcing](/docs/active-directory/initial-access/brute-force.html) or [password spraying](/docs/active-directory/initial-access/password-spraying.html) without risking to lockout accounts.