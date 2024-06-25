---
layout: default
title: Active Directory First Lookup
parent: Active Directory
nav_order: 6
permalink: /ad/lookup/
---

# Active Directory First Lookup
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites

- Network route to at least one domain controller.

## Exploit

First, let's identify domain controllers of the Active Directory domain you are targeting.

```bash
# Get domain name.
nmcli dev show $Interface | grep -i "domain"

# Get domain controllers IPs.
nslookup $DomainName
```

Now, using [NetExec](https://github.com/Pennyw0rth/NetExec), try to authenticate as anonymous and Guest.

```bash
# Get domain controllers system info.
nxc smb $DomainControllersIPs

# Authenticate as Guest and anonymous.
cme smb $DomainControllerIP -d $DomainName -u 'Guest' -p ''
cme smb $DomainControllerIP -d $DomainName -u 'Guest' -p '' --local-auth
cme smb $DomainControllerIP -d $DomainName -u '' -p ''
cme smb $DomainControllerIP -d $DomainName -u '' -p '' --local-auth
```