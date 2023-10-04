# Methodologie Web

## Fuzzing

### DirB

```bash
dirb http://$ip/ /path/to/wordlist
```

### Ffuf

```bash
ffuf -w /path/to/wordlist -u https://$ip/FUZZ
```

### GoBuster

```bash
gobuster dir -u https://$ip/ -w /path/to/wordlist
```
