# Processes
---

```powershell
# List running processes.
tasklist /FI "STATUS eq RUNNING"
wmic process get ProcessID,ExecutablePath
Get-Process
ps
```