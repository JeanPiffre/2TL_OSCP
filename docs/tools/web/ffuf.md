## Présentation

Ffuf (Fuzz Faster U Fool) est un outil rapide et efficace pour le fuzzing, qui aide les professionnels de la sécurité et les développeurs à découvrir des vulnérabilités dans les systèmes et applications Web. Il peut être utilisé pour identifier des répertoires, des scripts, des fichiers et d'autres ressources cachés sur un site Web ou un serveur. Grâce à sa conception modulaire et sa capacité à effectuer des requêtes HTTP rapides, Ffuf est capable de fournir des résultats précis et exhaustifs en un temps record.

## Installation

### Linux 

=== "Debian / Ubuntu"

    ``` bash
    wget https://github.com/ffuf/ffuf/releases/download/v1.3.1/ffuf_1.3.1_linux_amd64.tar.gz
    tar xvzf ffuf_1.3.1_linux_amd64.tar.gz
    ```

## Utilisations

### Recherche de Répertoires Cachés

```bash
ffuf -w /path/to/wordlist -u https://$ip/FUZZ
```