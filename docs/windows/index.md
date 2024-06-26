---
layout: default
title: Windows
nav_order: 10
has_children: true
permalink: /windows/
has_toc: false
---

# Windows
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Without an account

First, you should perform a [Network Recon](/network/) of your target to identify quick wins or running services you could exploit. Once identified, refer to [Web Attacks](/webapps/) to attempt remote code execution that could give you an initial foothold on the target or try following exploits.

- [BlueKeep [CVE-2019-0708]](/windows/bluekeep/)
- [EternalBlue [MS17-010]](/windows/eternalblue/)
- [Evade Kiosk Mode](/windows/kiosk/)
- [SNMP Enumeration](/network/snmp/)

More initial foothold exploits are be possible if your target is joined to an [Active Directory](/ad/#without-an-account) domain.

## Privilege escalation

Once you get a low privileged access on a Windows target, you can perform following attacks to escalate your privileges locally. You should perform [Windows Enumeration](/windows/enumeration/) to get as much information as possible before starting to exploit.

- [Binary Abuse](/windows/binary/)
- [Cached Credentials Extraction](/windows/credentials/)
- [Microsoft Office Exploit](/windows/office/)
- [SeImpersonatePrivilege Abuse](/windows/seimpersonateprivilege/)
- [Service Hijacking](/windows/service/)
- [SpoolFool [CVE-2022-21999]](/windows/spoolfool/)
- [Unquoted Service Paths](/windows/unquoted/)

More privilege escalation exploits are possible if your target is joined to an [Active Directory](/ad/#privilege-escalation) since you can authenticate on the domain from the machine.

## Lateralization

- [DCOM Abuse](/windows/abuse/)
- [Data Protection API Abuse](/windows/dpapi/)
- [PSExec Abuse](/windows/psexec/)
- [WinRM Abuse](/windows/winrm/)

More lateralization exploits are be possible if your target is joined to an [Active Directory](/ad/#lateralization).