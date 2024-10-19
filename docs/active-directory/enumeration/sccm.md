# System Center Configuration Manager
---

If following commands returns something, take a look at [SCCM Abuse](/ad/sccm/) attack.

```powershell
Get-WmiObject -Class SMS_Authority -Namespace root\CCM | select Name, CurrentManagementPoint | Format-List
```