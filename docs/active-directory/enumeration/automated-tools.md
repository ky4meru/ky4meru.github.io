# ⚙️ Automated Tools
---

#### BloodHound

The best tool to enumerate and get graphical insights of an Active Directory domain is [BloodHound](https://github.com/BloodHoundAD/BloodHound) and associated ingestors like [BloodHound.py](https://github.com/dirkjanm/BloodHound.py) or [SharpHound](https://github.com/BloodHoundAD/SharpHound).

First, you will need to extract all objects of the domain. These ingestors will do it and will generate multiple JSON files containing domain's objects. You can now import these files in [BloodHound](https://github.com/BloodHoundAD/BloodHound) to visualize domain's organization and find attack paths.

# [Linux](#tab/linux)

```bash
bloodhound-python -d $domain -u $username -p $password -c All
```

# [Windows](#tab/windows)


```powershell
# From a domain joined Windows computer.
.\SharpHound.exe -c All

# From a Windows computer which is not joined to the domain.
.\SharpHound.exe -d $Domain --ldapusername $Username --ldappassword $Password -c All
```

> [!NOTE]
> You might be connected on a domain joined computer with a local account. In such a case, you won't be able to start `SharpHound.exe` from a PowerShell started with this local account.
> To bypass this, you must run `SharpHound.exe` from a PowerShell started as `NT AUTHORITY\SYSTEM` so `SharpHound.exe` will use domain computer credentials to authenticate on the Active Directory domain.
> To do so, you can use [PSExec](https://learn.microsoft.com/en-us/sysinternals/downloads/psexec) by running `Psexec.exe -i -s C:\WINDOWS\system32\WindowsPowerShell\v1.0\powershell.exe`.

---

Once you ingested domain's objects, start BloodHound and import JSON files.

```bash
# On first use, go on http://localhost:7474/ to set neo4j credentials (default are neo4j:neo4j).
sudo neo4j start

# Enter neo4j credentials.
bloodhound
```

#### PingCastle

[PingCastle](https://www.pingcastle.com/) is also a very good candidate to enumerate an Active Directory domain. It will provide risks insights, highlighting misconfigurations and recommendations to apply.

```powershell
# From the domain controller itself.
.\PingCastle.exe --healthcheck

# From a distant (workgroup or domain joined) computer.
.\PingCastle.exe --healthcheck --server $Domain --user $Username --password $Password
```

#### PowerView and SharpView

PowerView from [PowerSploit](https://github.com/PowerShellMafia/PowerSploit/) framework is a PowerShell module that provides many builtins to enumerate an Active Directory domain.

```powershell
powershell -ep bypass
Import-Module ./PowerView.ps1
Get-DomainController -Domain $Domain
```

[SharpView](https://github.com/tevora-threat/SharpView) is a .NET port for PowerView, with the same features.

```powershell
.\SharpView.exe Get-DomainController -Domain $Domain
.\SharView.exe $Command $Options
```

#### ADSearch

[ADSearch](https://github.com/tomcarver16/ADSearch) allows to perform custom LDAP queries.

```powershell
.\ADSearch.exe --search "$LDAPFilter"
```

#### ADRecon

[ADRecon](https://github.com/adrecon/ADRecon) will do a quick enumeration of basic information about an Active Directory domain.

```powershell
# From a domain joined computer.
.\ADRecon.ps1

# From a workgroup computer.
$SecurePassword = ConvertTo-SecureString "$Password" -AsPlainText -Force
$Credentials = New-Object -TypeName System.Management.Automation.PSCredentials -ArgumentList "$Domain\$Username", $SecurePassword
.\ADRecon.ps1 -DomainController $DomainControllerIP -Credential $Credentials
```