---
description: "Initialiser un nouvel audit de sécurité pour un projet en copiant le template de base"
allowed-tools: Bash, Read, Glob, Write, AskUserQuestion
---

# Initialisation d'un audit de sécurité

Tu dois initialiser un nouvel espace d'audit pour un projet en copiant le template de base `.audit-securite/`.

## Arguments

L'argument fourni est : `$ARGUMENTS`

Le format attendu est : `<nom-du-projet> <chemin-du-projet>`

Exemples :
- `mon-app /home/user/dev/mon-app`
- `api-backend /home/user/projets/api-backend`

Si les arguments sont incomplets, demande à l'utilisateur :
1. Le **nom** de l'audit (sera utilisé comme nom de dossier)
2. Le **chemin absolu** du projet à auditer

## Actions

1. Crée le dossier `<nom-du-projet>/` à la racine du répertoire de travail
2. Copie l'intégralité du contenu de `.audit-securite/` dans `<nom-du-projet>/`
3. Vérifie que la copie est complète (tous les fichiers de recommandations + suivi.md)

```bash
cp -r .audit-securite/* <nom-du-projet>/
```

4. Affiche un résumé :

```
Audit initialisé pour : <nom-du-projet>
Projet cible         : <chemin-du-projet>
Dossier d'audit      : <nom-du-projet>/
Recommandations      : 63 (21 catégories)

Prochaines étapes :
  1. /scan-projet <nom-du-projet> <chemin-du-projet>
  2. /audit-securite <nom-du-projet> <chemin-du-projet>
```

## Répertoire de travail

Le répertoire de base est celui où se trouve `.audit-securite/`.
Chaque projet audité a son propre dossier à la racine, au même niveau que `.audit-securite/`.
