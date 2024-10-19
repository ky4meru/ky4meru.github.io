# AppLocker
---

If AppLocker is enabled, take a look at some ways to [bypass](/ad/applocker/) it.

```powershell
reg query "HKLM\Software\Policies\Microsoft\Windows\SrpV2"
Get-ChildItem "HKLM:Software\Policies\Microsoft\Windows\SrpV2"
Get-AppLockerPolicy -Effective -XML
Get-AppLockerFileInformation | Format-List
Get-DomainGPO -Domain dev-studio.com | ? { $_.DisplayName -like "*AppLocker*" } | select displayname, gpcfilesyspath
```