# 🔍 Find Domain Name
---

#### 🤓 RTFM

Once you plug your computer on a company's network, you generally got an IP address assigned through the [DHCP](https://techterms.com/definition/dhcp) protocol. It sends a lot of useful information including domain names that are set for you.

#### 🛠️ Do it!

The method leveraging `nmap` can be performed from either Linux or Windows.

# [Nmap](#tab/nmap)

[Nmap](https://nmap.org/) is a cross platform tool that can give you a lot of useful information.

```shell
nmap --script broadcast-dhcp-discover
```

# [Linux](#tab/linux)

You can also check `/etc/resolf.conf` which stores name resolution configuration.

```bash
# Once plugged on the target's network, identify the corresponding network interface with 'ip a' command.
network_interface=eth0

# Get Active Directory domain name.
domain_name=$(nmcli dev show $network_interface | grep -i "domain" | awk -F' ' '{print $2}')
```

# [Windows](#tab/windows)

```powershell
$DomainName = Get-WmiObject -Namespace root\cimv2 -Class Win32_ComputerSystem | Select-Object -Property Domain
```

---

#### 🏆 What's next?

Now you know the domain name, try to spot [domain controllers](/docs/active-directory/recon/domain-controllers.html).