# 🎯 Windows
---

#### 🔑 Initial access

First, you should perform a [Network Recon](/network/) of your target to identify quick wins or running services you could exploit. Once identified, refer to [Web Attacks](/webapps/) to attempt remote code execution that could give you an initial foothold on the target or try following exploits.

- [BlueKeep [CVE-2019-0708]](/docs/windows/initial-access/bluekeep.html)
- [EternalBlue [MS17-010]](/docs/windows/initial-access/eternalblue.html)
- [Evade Kiosk Mode](/docs/windows/initial-access/kiosk-mode-evasion.html)

More initial foothold exploits are be possible if your target is joined to an [Active Directory](/ad/#without-an-account) domain.

#### 🔓 Enumeration

- [Automated Enumeration](/docs/windows/enumeration/automated-tools.html)
- [Environment Variables](/docs/windows/enumeration/environment-variables.html)
- [Files](/docs/windows/enumeration/files.html)
- [Network Configuration](/docs/windows/enumeration/network-configuration.html)
- [PowerShell History](/docs/windows/enumeration/powershell-history.html)
- [Processes](/docs/windows/enumeration/processes.html)
- [Services](/docs/windows/enumeration/services.html)
- [Softwares](/docs/windows/enumeration/softwares.html)
- [System Info](/docs/windows/enumeration/system-info.html)
- [Users](/docs/windows/enumeration/users.html)

#### 💣 Privilege escalation

Once you get a low privileged access on a Windows target, you can perform following techniques to escalate your privileges locally - *i.e. gain local administrator privileges*.

- [AppLocker](/windows/binary/)
- [Credentials Extraction](/windows/credentials/)
- [Microsoft Office Exploit](/windows/office/)
- [SeImpersonatePrivilege Abuse](/windows/seimpersonateprivilege/)
- [Service Hijacking](/windows/service/)
- [SpoolFool [CVE-2022-21999]](/windows/spoolfool/)
- [Unquoted Service Paths](/windows/unquoted/)

More privilege escalation exploits are possible if your target is joined to an [Active Directory](/ad/#privilege-escalation) since you can authenticate on the domain from the machine.

#### ⚔️ Lateralization

- [DCOM Abuse](/windows/abuse/)
- [Data Protection API Abuse](/windows/dpapi/)
- [PSExec Abuse](/windows/psexec/)
- [WinRM Abuse](/windows/winrm/)

More lateralization exploits are be possible if your target is joined to an [Active Directory](/ad/#lateralization).