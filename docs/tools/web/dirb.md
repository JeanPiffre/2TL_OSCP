---
tags:
  - Tools
  - Web
  - Fuzzing
---

## Présentation

Dirb est un outil de balayage de contenu Web efficace et populaire. Il est principalement utilisé pour découvrir des fichiers et des répertoires cachés sur un site Web. Dirb travaille en lançant une série d'attaques par force brute contre un serveur Web et en analysant les réponses pour identifier les ressources potentiellement accessibles. Cela peut être crucial pour révéler des informations sensibles ou des points d'entrée vulnérables sur un site Web.

## Installation

### Linux 

=== "Debian / Ubuntu"

    ``` bash
    sudo apt-get update
    sudo apt-get install dirb
    ```

## Utilisations

### Recherche de Répertoires Cachés

```bash
dirb http://$ip/ /path/to/wordlist
```