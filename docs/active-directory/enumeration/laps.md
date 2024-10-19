## Local Administrator Password Solution
---

If LAPS is enabled, you could [abuse](/ad/laps/) its configuration to escalate your privileges.

```powershell
dir "C:\Program Files\LAPS\CSE\"
Get-DomainComputer | ? { $_."ms-Mcs-AdmPwdExpirationTime" -ne $null } | select dnsHostName
Get-DomainGPO | ? { $_.DisplayName -like "*laps*" } | select DisplayName, Name, GPCFileSysPath | Format-List
```