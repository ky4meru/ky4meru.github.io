# 🖨️ Printer Spoofing
---

#### 🤓 RTFM

Printers are usually present within companies. Sometimes, to authenticate users, printers use LDAP requests against the domain. Therefore, the purpose of this technique is to gain access to the configuration web page of the printer as administrator to relay authentication requests to your environment instead to the domain.

#### 📝 Prerequisites

- Printer must use LDAP authentication against the target domain.

#### 🛠️ Do it!

First, you need to find printers on the network. To do so, start by scanning web ports with [Nmap](https://nmap.org/) or [Masscan](https://github.com/robertdavidgraham/masscan) to find printer's web administration pages. Then, you can use [Aquatone](https://github.com/michenriksen/aquatone) to facilitate your research.

```bash
# Scan open web ports on the network.
sudo masscan $IPRange -p80,443,8000,8008,8080,8443 -oX out.xml

# Put the result in aquatone to screenshot web pages.
cat out.xml | aquatone -out ./aquatone -nmap

# Look at the results.
feh ./aquatone/screenshots
```

If you find printer web pages, they might be protected with authentication (it is not always the case). **Do not panic, default credentials are often in use**. Try to search on the Internet default passwords depending on the printer brand. You can also search for the printer brand in [DefaultCreds-Cheat-Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet/blob/main/DefaultCreds-Cheat-Sheet.csv).

Once you are authenticated on the printer as an administrator, search for the LDAP configuration page. If the printer authenticates users on the domain, you should see a field somewhere that specifies the destination IP or DNS.

Change this field with your local IP and start a [Responder](https://github.com/lgandx/Responder) to intercept authentication requests.

```bash
# To get your network interface.
ip a

# Then start responder.
sudo responder -wrfbudPF -I $NetworkInterface
```

That's all, just wait for someone to use the printer you just compromise and you will get your initial foothold.

#### 🏆 What's next?

- If it is your first authenticated access to the domain, let's [enumerate](/docs/active-directory/methodology.html#-enumeration) it!
- [Active Directory privilege escalation](/docs/active-directory/methodology.html#-privilege-escalation) if you gained access to a low privileged domain account.
- [Active Directory persistence](/docs/active-directory/methodology.html#-persistence) if you found a privileged domain account credentials.
- Otherwise, try another method to get an [initial access](/docs/active-directory/methodology.html#-initial-access).