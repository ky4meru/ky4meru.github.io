# ⚙️ Automated Tools
---

> [!CAUTION]
> Don't forget to take a look at [Antivirus Enumeration](/antivirus/enumeration/) page to list active antivirus and EDRs on the Windows host before dropping any tool to avoid being detected! Otherwise, take your chance with [Manual Enumeration](/windows/enumeration/#manual-enumeration).

#### WinPwn

Using [WinPwn](https://github.com/S3cur3Th1sSh1t/WinPwn).

```powershell
Import-Module .\WinPwn.ps1
```

#### WinPEAS

Using [WinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS).

```powershell
.\winPEAS.exe
.\winPEAS.ps1
```
#### Seatbelt

Using [Seatbelt](https://github.com/GhostPack/Seatbelt).

```powershell
# Using Seatbelt locally.
Seatbelt.exe -group=all -full -outfile="C:\Temp\out.txt"

# Using Seatbelt remotely.
Seatbelt.exe -group=system-computername=$target -username=$domain\$username -password="$password"
```

#### JAWS

Using [JAWS](https://github.com/411Hall/JAWS).

```powershell
powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1 -OutputFilename JAWS-Enum.txt
```

#### Wes-NG

Using [Wes-NG](https://github.com/bitsadmin/wesng).

```powershell
systeminfo.exe > systeminfo.out
wes.py systeminfo.out
```

#### Sherlock and Watson

Using [Sherlock](https://github.com/rasta-mouse/Sherlock) and its successor [Watson](https://github.com/rasta-mouse/Watson).

```powershell
# With Sherlock.
Import-Module .\Sherlock.ps1
Find-MS10092

# With Watson.
.\Watson.exe
```