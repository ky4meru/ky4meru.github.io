# 🌬️ User Spraying
---

#### 🤓 RTFM

Kerberos protocol implementation makes possible to request a TGT as soon as you know the username of a domain user. If you get an AS-REP, it means the domain user exists. You can leverage this behavior to validate a user exists or not.

#### 📝 Prerequisites

* Network route to a domain controller.
* Kerberos protocol is enabled (port 88 by default).

#### 🛠️ Do it!

# [Kerbrute](#tab/kerbrute)

[Kerbrute](https://github.com/ropnop/kerbrute) is a cross platform tool to perform Kerberos pre-authentication brute-forcing.

```bash
kerbrute userenum -d $Domain --dc $DomainControllerIP usernames.txt
```

# [Nmap](#tab/nmap)

[Nmap](https://nmap.org/) cross platform tool provides a `krb5-enum-users` script that can be leveraged to perform user spraying.

```bash
nmap -p 88 --script="krb5-enum-users" --script-args="krb5-enum-users.realm='$Domain',userdb=usernames.txt" $DomainControllerIP
```

---

#### 🏆 What's next?

Now you know some domain usernames, let's try to [brute-force](/docs/active-directory/initial-access/brute-force.html) their password or to perform [password spraying](/docs/active-directory/initial-access/password-spraying.html).

If you - *unfortunately* - found nothing, try other [initial access](/docs/active-directory/methodology.html#-initial-access) techniques.

#### 🔗 Useful links

- If you want to check how [Kerberos](https://www.tarlogic.com/blog/how-kerberos-works/) works.