---
description: "Scanner un projet pour obtenir un aperçu complet de la stack, structure et configurations utiles à l'audit de sécurité ANSSI"
allowed-tools: Bash, Read, Glob, Grep, Task, Write, WebFetch, WebSearch
---

# Scan de projet - Apercu pour audit de securite

Tu es un auditeur de securite specialise dans la securite des applications web. Tu dois analyser le projet cible pour produire un rapport structure qui servira de base a l'audit des recommandations ANSSI (ANNX-3 - Securite cote navigateur).

## Arguments

L'argument fourni est : `$ARGUMENTS`

Le format attendu est : `<nom-du-projet> <chemin-du-projet>`

Exemples :
- `mon-app /home/user/dev/mon-app`
- `api-backend /home/user/projets/api-backend`

Si les arguments sont incomplets, demande a l'utilisateur le nom de l'audit et le chemin du projet.

## Repertoire d'audit

Le dossier d'audit du projet est : `<nom-du-projet>/` (a la racine du repertoire de travail).
Verifie que ce dossier existe (cree par `/init-audit`). S'il n'existe pas, indique a l'utilisateur de lancer `/init-audit` d'abord.

## Objectif

Produire un fichier `scan-projet.md` dans le dossier d'audit du projet (`<nom-du-projet>/scan-projet.md`).

## Methodologie de scan

Lance les analyses suivantes **en parallele** autant que possible :

### 1. Structure du projet
- Lister l'arborescence des dossiers principaux (2-3 niveaux de profondeur)
- Identifier les repertoires cles : source, config, infra, tests, assets, public/static

### 2. Stack technique
- **Langages** : identifier les langages utilises (extensions de fichiers, fichiers de config)
- **Frameworks** : detecter les frameworks (package.json, pom.xml, build.gradle, Gemfile, requirements.txt, composer.json, go.mod, Cargo.toml, etc.)
- **Serveur web** : nginx, Apache, Tomcat, Express, etc.
- **Base de donnees** : type de BDD utilisee
- **Infrastructure** : Docker, Kubernetes, Cloud (AWS/GCP/Azure), CI/CD

### 3. Configuration de securite transport (TLS/HTTPS)
- Chercher les configurations SSL/TLS (certificats, redirections HTTPS, HSTS)
- Configurations ingress/reverse proxy
- Ports exposes

### 4. Configuration des en-tetes HTTP de securite
- Chercher les en-tetes de securite configures :
  - `Content-Security-Policy`
  - `Strict-Transport-Security`
  - `X-Content-Type-Options`
  - `X-Frame-Options`
  - `Referrer-Policy`
  - `Permissions-Policy`
  - `Cross-Origin-Opener-Policy`
  - `Cross-Origin-Resource-Policy`
  - `Cross-Origin-Embedder-Policy`
- Identifier **ou** ces en-tetes sont definis (serveur web, reverse proxy, application, meta tags)

### 5. Gestion des cookies
- Chercher les configurations de cookies (flags HttpOnly, Secure, SameSite, Path, Domain)
- Configuration des sessions (session cookies, duree, stockage)

### 6. JavaScript et contenus dynamiques
- Identifier l'utilisation de `eval()`, `innerHTML`, `document.write`, `Function()`, `setTimeout/setInterval` avec strings
- Chercher les usages de `postMessage`, `addEventListener('message')`
- Identifier les Web Workers
- Chercher les iframes et leur configuration (sandbox, allow)
- Chercher les usages de `document.domain`
- Chercher les usages de JSON-P

### 7. Requetes cross-origin et API
- Configuration CORS (Access-Control-Allow-Origin, etc.)
- Usages de XMLHttpRequest et Fetch API
- Tokens CSRF

### 8. Stockage cote client
- Usages de localStorage, sessionStorage
- Usages de IndexedDB
- Usages de Web SQL Database

### 9. Integrite des ressources
- Verifier la presence de Subresource Integrity (SRI) sur les scripts/styles externes
- Identifier les CDN et ressources tierces chargees

### 10. Composants tiers
- Lister les dependances (et leurs versions si possible)
- Identifier les bibliotheques JavaScript tierces chargees cote client

### 11. Deploiement
- Configuration des environnements (dev, staging, prod)
- Profils de deploiement
- Variables d'environnement liees a la securite

## Format de sortie

Ecris le rapport dans `<nom-du-projet>/scan-projet.md` avec le format suivant :

```markdown
# Scan de projet — [Nom du projet]
> Date du scan : YYYY-MM-DD
> Chemin : [chemin du projet]

## 1. Structure du projet
[Arborescence simplifiee]

## 2. Stack technique
| Composant | Technologie | Version | Fichier de reference |
|-----------|------------|---------|---------------------|
| Langage | ... | ... | ... |
| Framework | ... | ... | ... |
| Serveur web | ... | ... | ... |
| BDD | ... | ... | ... |
| CI/CD | ... | ... | ... |

## 3. Transport et TLS
[Resume des configurations TLS/HTTPS trouvees]
### Fichiers pertinents
- `chemin/fichier:lignes` — description

## 4. En-tetes HTTP de securite
| En-tete | Present | Valeur | Fichier |
|---------|---------|--------|---------|
| Content-Security-Policy | Oui/Non | ... | ... |
| ... | ... | ... | ... |

## 5. Cookies et sessions
[Configuration trouvee]
### Fichiers pertinents
- ...

## 6. JavaScript et contenus dynamiques
### Pratiques a risque detectees
| Pattern | Occurrences | Fichiers |
|---------|------------|---------|
| eval() | N | ... |
| innerHTML | N | ... |
| ... | ... | ... |

### Communication inter-contextes
[postMessage, iframes, Web Workers...]

## 7. Cross-origin et API
[CORS, XHR, Fetch, CSRF...]

## 8. Stockage cote client
[localStorage, sessionStorage, IndexedDB...]

## 9. Integrite des ressources
[SRI, CDN, ressources tierces...]

## 10. Composants tiers
[Dependances principales avec versions]

## 11. Deploiement et environnements
[Profils, variables, configurations...]

## 12. Points d'attention pour l'audit
[Liste des elements identifies qui necessitent une verification approfondie]
```

## Instructions importantes

- Ne modifie AUCUN fichier du projet cible. Ce scan est en **lecture seule**.
- Sois exhaustif dans ta recherche mais concis dans le rapport.
- Pour chaque element trouve, indique toujours le **chemin du fichier et les lignes** concernees.
- Si un element n'est pas trouve, indique-le explicitement (c'est une information utile pour l'audit).
- Utilise des agents en parallele pour accelerer le scan.
