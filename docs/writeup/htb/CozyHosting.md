---
tags:
  - Easy
  - Linux
---

# CozyHosting

![CozyHosting](img_CozyHosting/logo.png)

## Information Générales

- **Difficulté** : Easy
- **Système d'exploitation** : Linux


## Walkthrough

### Hosts

```bash
echo "$ip cozyhosting.htb" | sudo tee -a /etc/hosts
```

### Enumeration

#### Nmap

```bash
$ nmap 10.129.98.140
Starting Nmap 7.93 ( https://nmap.org ) at 2023-10-04 11:12 BST
Nmap scan report for 10.129.98.140
Host is up (0.069s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

PORT 22 - SSH

PORT 80 - HTTP

