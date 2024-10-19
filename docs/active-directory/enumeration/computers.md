# Computers
---

```bash
# Considering ComputersIPs.txt contains a list of IPs, one per line.
# $DNSServer should correspond in most cases to the domain controller IP.
for IP in $(cat ComputersIPs.txt); do host $IP $DNSServer; done > ComputerNames.txt
```