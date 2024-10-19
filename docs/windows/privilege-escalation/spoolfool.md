# SpoolFool [CVE-2022-21999]
---

## Vulnerability

This vulnerability allows to load arbitrary files as DLLs through the print spool service of Windows and execute them with administrative privileges.

More details [here](https://www.logpoint.com/en/blog/a-spools-gold-cve-2022-21999-yet-another-windows-print-spooler-privilege-escalation-2/).

## Prerequisites

* Having a local low privileged access to the targeted machine.

## Exploit

To exploit this vulnerability, you should use [Ly4k's repository](https://github.com/ly4k/SpoolFool). By default, the provided DLL in his repository will create a new local administrator `admin:Passw0rd!`, but you are free to use another or your own DLL.

```powershell
.\SpoolFool.exe -dll add_user.dll
```

{: .warning }
As mentioned [here](https://github.com/ly4k/SpoolFool?tab=readme-ov-file#artifacts), don't forget to cleanup artifacts after the exploit.

## Recommendations

- [ ] Apply the [security patch](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2022-21999) provided by Microsoft on vulnerable machines.