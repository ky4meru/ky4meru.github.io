# Kiosk Mode Evasion
---

#### 🤓 RTFM

In some situations, you might face a Windows hardened as a kiosk to prevent any illegitimate usage of the target operating system. It's also possible to find this kiosk mode on VDI (Virtual Desktop Infrastructure) like Citrix XenDesktop or XenApp. In such a situation, your goal would be to evade the kiosk mode by poping up an interactive `cmd.exe` or `PowerShell` to gain an initial access to the Windows system and then try to [escalate your privileges](/windows/#privilege-escalation).

#### 📝 Prerequisites

* Target must be Windows based with a kiosk mode enforced.

#### 🛠️ Do it!

If the kiosk mode blocks you in an application, try this checklist:

- [ ] Import or Export feature to open Windows Explorer.
- [ ] Print feature to use `Print to file` feature that will open Windows Explorer.
- [ ] Search feature like an URL top bar that lookups locally on the system.

If the kiosk mode blocks you on the operating system, try this checklist:

```bash
# Type Windows key (or Windows key + R) then...
runas /netonly /user:fake powershell

# Create a Windows shortcut with following target
C:\Windows\System32\cmd.exe /k "start powershell"
```

#### 🏆 What's next?

TODO