# Services
---

```powershell
# List all services, including stopped ones.
sc query state= all
Get-Service
Get-CimInstance -ClassName win32_service | Where-Object {$_.State -like "Running"}

# List running services.
sc query
Get-Service | Where-Object {$_.Status -eq "Running"}
Get-CimInstance -ClassName win32_service | Where-Object {$_.State -like "Running"}

# Get a specific service.
sc query $ServiceName
Get-Service $ServiceName
Get-CimInstance -ClassName win32_service | Where-Object {$_.Name -like "$ServiceName"}

# Get executable name executed by a service.
tasklist /fi "SERVICES eq $ServiceName"

# Get full path of an executable executed by a service.
wmic process where "name='$ExecutableName'" get ExecutablePath
```

