---
tags:
  - Tools
  - Web
  - Fuzzing
---

## Présentation

Dirsearch est un outil de scan de répertoires Web conçu pour aider les professionnels de la sécurité à identifier et à localiser des ressources et des chemins cachés dans les applications et les sites Web. Il fonctionne en utilisant des techniques de force brute, en testant une multitude de noms de répertoires et de fichiers contre le site Web cible et en observant les réponses pour identifier les éléments existants. Dirsearch est apprécié pour sa facilité d'utilisation et sa flexibilité, permettant une configuration et une personnalisation détaillées pour s'adapter à divers scénarios et environnements de test.

## Installation

### Linux 

=== "Debian / Ubuntu"

    ``` bash
    git clone https://github.com/maurosoria/dirsearch
    ```

## Utilisations

### Recherche de Répertoires Cachés

```bash
cd dirsearch
python3 dirsearch.py -u <URL>
```