## Présentation

GoBuster est un outil écrit en Go qui permet aux testeurs de pénétration et aux administrateurs de systèmes de découvrir des répertoires et des fichiers cachés sur un site Web ou un serveur. GoBuster utilise des attaques par force brute pour identifier et exposer des ressources qui ne sont pas visibles pour les utilisateurs ordinaires. Il est apprécié pour sa rapidité et sa flexibilité, permettant aux professionnels de personnaliser les attaques en fonction de leurs besoins spécifiques et des caractéristiques du système cible.

## Installation

### Linux 

=== "Debian / Ubuntu"

    ``` bash
    go get github.com/OJ/gobuster
    wget https://github.com/OJ/gobuster/releases/download/v3.1.0/gobuster-linux-amd64.7z
    7z x gobuster-linux-amd64.7z
    ```

## Utilisations

### Recherche de Répertoires Cachés

```bash
gobuster dir -u https://$ip/ -w /path/to/wordlist
```
