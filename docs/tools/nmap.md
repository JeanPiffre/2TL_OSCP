## Présentation

Bien sûr! Nmap (Network Mapper) est un outil open source pour l'exploration réseau et l'audit de sécurité. Il a été conçu pour découvrir les hôtes et les services sur un réseau informatique, créant ainsi une "carte" du réseau. Nmap est incroyablement puissant et flexible, avec des capacités qui incluent la découverte d'hôtes, le scan de ports, la détection de système d'exploitation, la détection de version de service, et bien d'autres.

## Installation

### Linux 

=== "Debian / Ubuntu"

    ``` bash
    sudo apt update
    sudo apt install nmap
    ```

=== "RedHat / CentOS"

    ```bash
    sudo yum install nmap
    ```

=== "Fedora"

    ```bash
    sudo dnf install nmap
    ```

### Windows
- Visitez le site Web de Nmap : https://nmap.org/download.html
- Téléchargez le fichier d'installation pour Windows.
- Exécutez le fichier d'installation et suivez les instructions pour installer Nmap.

### Mac

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install nmap
```

## Utilisations

### Stealth scan using SYN

```bash
nmap -oN nmap2.txt -v -sU -sS -p- -A -T4 $ip
```

### Basic TCP Scan with Output

```bash
nmap -sC -sV -oA $ip
```

### Verbose TCP Open Ports Scan

```bash
nmap -v --open -sC -T4 -oA $ip
```

### Detailed TCP Scan with Grepable Output

```bash
nmap -T4 -sC -sV -oO --open -v $ip
```

### Scan service detection

```bash
nmap -sS -sC -sV -Pn -p- -T4 -A $ip
```