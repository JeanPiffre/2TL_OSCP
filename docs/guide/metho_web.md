# Methodologie Web

## Hosts

Si le site n'est pas accessible, ajouter l'adresse dans /etc/hosts

```bash
echo "$ip domaine.extension" | sudo tee -a /etc/hosts
```

## Fuzzing

### Outils

=== "DirB"

```bash
dirb http://$ip/ /path/to/wordlist
```

=== "Ffuf"

```bash
ffuf -w /path/to/wordlist -u https://$ip/FUZZ
```

=== "GoBuster"

```bash
gobuster dir -u https://$ip/ -w /path/to/wordlist
```

### Fichier Interessant 

- Fichier de **sessions**
-