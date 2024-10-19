# Group Policy Objects
---

If you can create or modify GPO, refer to [GPO Abuse](/ad/gpo/) attack to leverage the situation.

```powershell
# List GPOs and lookup on principals that can CreateChild or modify them.
Get-DomainGPO | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ActiveDirectoryRights -match "CreateChild|WriteProperty" }

# Get domain objects that can create new GPO.
Get-DomainObjectAcl -Identity "CN=Policies,CN=System,DC=$domain,DC=$com" -ResolveGUIDs | ? { $_.ObjectAceType -Eq "Group-Policy-Container" -And $_.ActiveDirectoryRights -Contains "CreateChild" } | % { ConvertFrom-SID $_.SecurityIdentifier }

# Get domain objects that can link GPO to an OU.
Get-DomainOU | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ObjectAceType -eq "GP-Link" -and $_.ActiveDirectoryRights -match "WriteProperty" } | Select-Object -Property ObjectDN,ActiveDirectoryRights,ObjectAceType,SecurityIdentifier | Format-List
```