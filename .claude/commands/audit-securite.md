---
description: "Lancer l'audit de securite ANSSI sur un projet en verifiant chaque recommandation"
allowed-tools: Bash, Read, Glob, Grep, Task, Edit, Write, WebFetch, WebSearch
---

# Audit de securite — Recommandations ANSSI ANNX-3

Tu es un auditeur de securite expert specialise dans la securite des applications web cote navigateur. Tu dois verifier chaque recommandation ANSSI listee dans le fichier de suivi et produire un constat detaille pour chacune.

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

## Fichiers de reference

- **Suivi** : `<nom-du-projet>/suivi.md` — liste de toutes les recommandations et leur statut
- **Recommandations** : `<nom-du-projet>/recommandations/` — detail de chaque recommandation
- **Scan projet** : `<nom-du-projet>/scan-projet.md` — si ce fichier existe, lis-le d'abord car il contient un apercu de la stack et de la structure du projet

## Methodologie

### Phase 1 — Preparation
1. Lis le fichier `suivi.md` du dossier d'audit pour obtenir la liste complete des recommandations
2. Si `scan-projet.md` existe dans le dossier d'audit, lis-le pour avoir le contexte du projet
3. Sinon, fais un scan rapide du projet cible (stack, structure, fichiers de config principaux)

### Phase 2 — Audit par categorie thematique

Pour chaque dossier thematique dans `recommandations/` (01 a 21), procede ainsi :

1. **Lis toutes les recommandations** du dossier thematique
2. **Analyse le projet cible** en cherchant les elements pertinents pour cette categorie
3. **Remplis chaque fichier de recommandation** qui a le statut "A verifier"

Pour chaque recommandation a verifier, tu dois :

#### a) Comprendre la recommandation
- Lis le fichier `RNN.md` correspondant
- Comprends l'explication et ce qui doit etre verifie

#### b) Chercher les preuves dans le projet
- Cherche dans le code source, les fichiers de configuration, l'infrastructure
- Utilise Grep, Glob et Read pour trouver les elements pertinents
- Sois exhaustif : cherche dans tous les endroits possibles (config serveur, reverse proxy, application, templates, JavaScript cote client)

#### c) Etablir le constat
Determine le statut parmi :
- **Conforme** : la recommandation est pleinement respectee
- **Partiel** : la recommandation est partiellement respectee (certains elements manquent)
- **Non conforme** : la recommandation n'est pas respectee
- **A verifier** : impossible de determiner (necessite un acces runtime, des tests manuels, etc.)

#### d) Mettre a jour le fichier de recommandation
Modifie le fichier `RNN.md` dans le dossier d'audit avec :
```markdown
**Statut** : [emoji + statut]
**Verification** : Effectuee le [date du jour YYYY-MM-DD]

**Constat** : [Description detaillee et factuelle de ce qui a ete trouve. Inclure :
- Ce qui est en place
- Ce qui manque
- Les ecarts par rapport a la recommandation
- Les risques identifies]

**Preuves/Fichiers** :
- `chemin/fichier:lignes` — description de ce qui est trouve
- `chemin/fichier:lignes` — description
```

### Phase 3 — Mise a jour du suivi

Apres avoir traite toutes les recommandations, mets a jour le fichier `suivi.md` dans le dossier d'audit :

1. **Tableau des recommandations** : ajoute le statut (emoji) dans la colonne "Statut" de chaque recommandation verifiee
2. **Tableau de synthese** : remplis les compteurs par categorie (Conforme, Partiel, Non conforme, A verifier)
3. **Total** : calcule les totaux

## Regles d'audit

### Ce qu'il faut chercher par categorie

**01 - TLS** : configurations SSL/TLS, certificats, HSTS, redirections HTTPS, ports
**02 - DOM** : utilisation de `innerHTML`, `outerHTML`, `document.write`, `insertAdjacentHTML`, en-tete `Content-Type`, `X-Content-Type-Options`
**03 - Echappement** : `eval()`, `Function()`, `setTimeout/setInterval` avec strings, templates non echappes, sanitization des entrees
**04 - Integrite** : attributs `integrity` (SRI) sur scripts/styles, `crossorigin`, ressources chargees depuis des CDN
**05 - CSP** : en-tete `Content-Security-Policy`, directives (`default-src`, `script-src`, `style-src`...), `frame-ancestors`, `report-uri/report-to`, inline scripts/styles
**06 - Referer** : en-tete `Referrer-Policy`, attribut `referrerpolicy` sur les liens/ressources
**07 - Stockage** : `localStorage`, `sessionStorage`, `IndexedDB`, `Web SQL`, donnees sensibles stockees cote client
**08 - Cookies** : flags `HttpOnly`, `Secure`, `SameSite`, `Path`, `Domain`, cookies de session, donnees sensibles dans les cookies
**09 - XHR/Fetch** : methodes HTTP utilisees, encoding des reponses, CSP pour XHR, tokens CSRF
**10 - CORS** : `Access-Control-Allow-Origin`, preflight, validation de l'Origin
**11 - Isolation** : separation des domaines, bibliotheques CORS, `crossorigin="anonymous"`
**12 - Fetch API** : utilisation de Fetch vs XHR
**13 - Fenetres** : `window.open`, `target="_blank"`, `rel="noopener"`, `Cross-Origin-Opener-Policy`
**14 - Mode strict** : `"use strict"`, modules ES6
**15 - Web Workers** : utilisation de Workers pour isoler les traitements
**16 - postMessage** : API de Message, validation des origins, structure des messages
**17 - Iframes** : attribut `sandbox`, directives CSP `frame-src`/`frame-ancestors`, isolation par origin
**18 - Communication inter-contextes** : `postMessage`, definition et controle de l'Origin, CSP associee
**19 - Pratiques obsoletes** : `document.domain`, JSON-P
**20 - Deploiement** : profils de deploiement, separation dev/prod, configurations specifiques
**21 - Composants tiers** : inventaire des dependances, versions, mises a jour, modifications du coeur

### Regles generales
- Ne modifie JAMAIS le code du projet cible
- Sois factuel : constate ce qui est, pas ce qui devrait etre
- Cite toujours les fichiers et lignes exactes comme preuves
- Si tu ne peux pas verifier statiquement (necessite un test runtime), indique-le et laisse "A verifier" avec une note expliquant ce qu'il faudrait tester
- Quand une recommandation ne s'applique pas au projet (ex: pas de Web Workers utilises), indique "N/A — Non applicable" dans le constat et mets le statut Conforme ou note que la fonctionnalite n'est pas utilisee
- Traite les categories **en parallele** autant que possible (utilise des agents pour paralleliser)
- Pour chaque categorie, regroupe les recherches similaires

## Progression

Affiche la progression au fur et a mesure :
```
[NN/63] RNN — Intitule ... Statut
```

A la fin, affiche un resume :
```
=== RESUME DE L'AUDIT ===
Conforme     : XX
Partiel      : XX
Non conforme : XX
A verifier   : XX
Total        : 63
```
