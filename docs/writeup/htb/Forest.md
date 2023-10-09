---
tags:
  - Easy
  - Windows
---

# CozyHosting

![Forest](img_Forest/logo.png)

## Information Générales

- **Difficulté** : Easy
- **Système d'exploitation** : Windows


## Walkthrough

### Enumeration

#### Nmap

```bash
$ nmap -sV $ip
Starting Nmap 7.93 ( https://nmap.org ) at 2023-10-09 09:36 BST
Nmap scan report for 10.129.92.214
Host is up (0.081s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT     STATE SERVICE      VERSION
53/tcp   open  domain       Simple DNS Plus
88/tcp   open  kerberos-sec Microsoft Windows Kerberos (server time: 2023-10-09 08:43:07Z)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds Microsoft Windows Server 2008 R2 - 2012 microsoft-ds (workgroup: HTB)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows
```

Domain controller : htb.local
LDAP (389/TCP)
SMB (445/TCP)

##### LDAP Enumeration

```bash
$ nmap -n -sV --script "ldap* and not brute" $ip
```

ou

```bash
$ ldapsearch -H ldap://$ip -x -s base namingcontexts
```

```bash
$ ldapsearch -H ldap://$ip -x -s base namingcontexts
# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: namingcontexts 
#

#
dn:
namingContexts: DC=htb,DC=local
namingContexts: CN=Configuration,DC=htb,DC=local
namingContexts: CN=Schema,CN=Configuration,DC=htb,DC=local
namingContexts: DC=DomainDnsZones,DC=htb,DC=local
namingContexts: DC=ForestDnsZones,DC=htb,DC=local

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```

DC=htb
DC=local


```bash
$ ldapsearch -H ldap://$ip -x -b DC=htb,DC=local
```

Il y a beacuoup d'inf, donc on va trier pour avoir que les comptes

```bash
$ ldapsearch -H ldap://$ip -x -b DC=htb,DC=local "(objectClass=person)" | grep "sAMAccountName:"
```