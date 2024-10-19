# 🔍 Find Domain Controllers
---

#### 🤓 RTFM

As explained by [The Hacker Recipes](https://www.thehacker.recipes/ad/recon/dns), *AD-DS (Active Directory Domain Services) rely on [DNS](https://techterms.com/definition/dns) SRV RR (service location resource records). Those records can be queried to find the location of some servers: the global catalog, [LDAP](https://techterms.com/definition/ldap) servers, the Kerberos KDC and so on.*

#### 🛠️ Do it!

Both methods through `nmap` or `nslookup` can be performed from either Linux or Windows.

# [Nmap](#tab/nmap)

[Nmap](https://nmap.org/) is a cross platform tool that can give you a lot of useful information.

```bash
nmap --script dns-srv-enum --script-args dns-srv-enum.domain=$DomainName
```

# [NsLookup](#tab/nslookup)

```bash
nslookup -type=srv _ldap._tcp.dc._msdcs.$DomainName
```

---

#### 🏆 What's next?

Now you discovered domain controller, try to find as much [domain computers](/docs/active-directory/recon/domain-computers.html) as you can.