# Audit de securite ANSSI — ANNX-3 (Securite cote navigateur)

Kit d'audit automatise base sur les recommandations de l'ANSSI pour la securite des applications web cote navigateur. Utilise [Claude Code](https://claude.ai/claude-code) pour analyser statiquement un projet et verifier la conformite aux 71 recommandations du referentiel.

## Prerequis

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installe et configure

## Demarrage rapide

Ouvrir Claude Code **depuis ce repertoire** :

```bash
cd /chemin/vers/secu-karim
claude
```

Puis executer les 3 commandes dans l'ordre :

```
/init-audit mon-app /home/user/dev/mon-app
/scan-projet mon-app /home/user/dev/mon-app
/audit-securite mon-app /home/user/dev/mon-app
```

## Commandes

Les 3 commandes sont des slash commands Claude Code (definies dans `.claude/commands/`).
Elles s'executent dans le chat Claude Code, pas dans un terminal.

Chaque commande prend **2 arguments separes par un espace** :

| Argument | Description | Exemple |
|----------|-------------|---------|
| `<nom-audit>` | Nom choisi pour cet audit (sera le nom du dossier de resultats) | `mon-app` |
| `<chemin-projet>` | Chemin absolu vers le projet a auditer | `/home/user/dev/mon-app` |

> Les deux arguments doivent etre **identiques** entre les 3 commandes pour que chaque etape retrouve le contexte de la precedente.

---

### Etape 1 — `/init-audit`

```
/init-audit <nom-audit> <chemin-projet>
```

**Ce qu'elle fait** : Cree un dossier `<nom-audit>/` a la racine de ce repertoire en copiant le template `.audit-securite/`. Ce dossier contiendra tous les resultats de l'audit.

**Resultat** : Un dossier `<nom-audit>/` avec `suivi.md` et 71 fiches de recommandations vierges.

**Prerequis** : Aucun.

---

### Etape 2 — `/scan-projet`

```
/scan-projet <nom-audit> <chemin-projet>
```

**Ce qu'elle fait** : Analyse le projet cible en lecture seule et produit un rapport `scan-projet.md` dans le dossier d'audit. Le scan couvre :

- Structure et arborescence du projet
- Stack technique (langages, frameworks, serveur, BDD, CI/CD)
- Configuration TLS/HTTPS et en-tetes de securite HTTP
- Cookies, sessions, stockage cote client
- Pratiques JavaScript a risque (innerHTML, eval, document.write, etc.)
- CORS, XHR/Fetch, CSRF
- Integrite des ressources et composants tiers
- Deploiement et environnements

**Resultat** : Fichier `<nom-audit>/scan-projet.md`.

**Prerequis** : Avoir execute `/init-audit` avec le meme `<nom-audit>`.

---

### Etape 3 — `/audit-securite`

```
/audit-securite <nom-audit> <chemin-projet>
```

**Ce qu'elle fait** : Verifie chaque recommandation ANSSI en cherchant les preuves dans le code du projet. Pour chacune des 71 recommandations :

1. Lit la fiche de recommandation pour comprendre ce qui doit etre verifie
2. Cherche les preuves dans le projet (code, config, infra)
3. Etablit un constat avec statut, description et fichiers preuves
4. Met a jour la fiche `RNN.md` et le tableau `suivi.md`

**Resultat** : Les 71 fiches `RNN.md` completees + `suivi.md` avec la synthese globale.

**Prerequis** : Avoir execute `/init-audit` et `/scan-projet` avec le meme `<nom-audit>`. Le scan n'est pas obligatoire mais il accelere significativement l'audit en fournissant le contexte du projet.

---

## Exemple complet

```
/init-audit preliq /home/negus/dev/newwinpaie
/scan-projet preliq /home/negus/dev/newwinpaie
/audit-securite preliq /home/negus/dev/newwinpaie
```

Apres execution, la structure generee :

```
preliq/
├── suivi.md                  # Tableau de suivi avec statuts et synthese
├── scan-projet.md            # Rapport de scan du projet
└── recommandations/
    ├── 01-TLS-et-transport-securise/
    │   ├── R01.md            # Fiche avec constat, statut et preuves
    │   ├── R02.md
    │   └── R03.md
    ├── 02-Composition-des-pages-et-DOM/
    │   ├── R04.md
    │   ├── R05.md
    │   └── R06.md
    ├── ...
    └── 21-Composants-logiciels-tiers/
        ├── R61.md
        ├── R62.md
        └── R63.md
```

## Structure du repertoire

```
.
├── .audit-securite/              # Template de base (ne pas modifier)
│   ├── suivi.md                  # Tableau de suivi vierge
│   └── recommandations/          # 71 fiches de recommandations
│       ├── 01-TLS-et-transport-securise/
│       ├── 02-Composition-des-pages-et-DOM/
│       ├── ...
│       └── 21-Composants-logiciels-tiers/
├── .claude/commands/             # Definitions des 3 commandes
│   ├── init-audit.md
│   ├── scan-projet.md
│   └── audit-securite.md
├── <nom-audit>/                  # Dossiers d'audit generes (un par projet)
└── README.md
```

## Categories de recommandations

| # | Categorie | Nb |
|---|-----------|---:|
| 01 | TLS et transport securise | 3 |
| 02 | Composition des pages et DOM | 3 |
| 03 | Echappement et validation des contenus | 4 |
| 04 | Integrite des ressources | 3 |
| 05 | Content Security Policy (CSP) | 8 |
| 06 | Politique Referer | 2 |
| 07 | Stockage local | 5 |
| 08 | Cookies | 8 |
| 09 | XMLHttpRequest (XHR) et Fetch | 7 |
| 10 | CORS | 2 |
| 11 | Isolation des services | 4 |
| 12 | API Fetch | 1 |
| 13 | Fenetres et navigation | 2 |
| 14 | Mode strict JavaScript | 1 |
| 15 | Web Workers | 2 |
| 16 | API de Message (postMessage) | 1 |
| 17 | Iframes et sandboxing | 4 |
| 18 | Communication inter-contextes | 4 |
| 19 | Pratiques obsoletes et dangereuses | 2 |
| 20 | Deploiement et configuration | 2 |
| 21 | Composants logiciels tiers | 3 |
| | **Total** | **71** |

## Statuts d'audit

| Emoji | Statut | Signification |
|-------|--------|---------------|
| ✅ | Conforme | Recommandation pleinement respectee |
| ⚠️ | Partiel | Partiellement respectee, des ecarts subsistent |
| ❌ | Non conforme | Non respectee ou absente |
| 🔍 | A verifier | Necessite une verification manuelle ou runtime |

## Format des fiches de recommandation

Chaque fiche `RNN.md` contient l'explication de la recommandation (template) puis, apres audit, le constat :

```markdown
# RNN — Intitule de la recommandation

**Explication** : Description technique de la recommandation

**Statut** : ✅ Conforme | ⚠️ Partiel | ❌ Non conforme | 🔍 A verifier
**Verification** : Effectuee le YYYY-MM-DD

**Constat** : Description factuelle de ce qui a ete trouve

**Preuves/Fichiers** :
- `chemin/fichier:lignes` — description
```

Les variantes `RNN-.md` (niveau degrade) et `RNN+.md` (niveau renforce) couvrent differents niveaux d'exigence pour une meme recommandation.

## Limitations

- L'audit est **statique** : il analyse le code source, les fichiers de configuration et l'infrastructure-as-code. Il ne teste pas le comportement runtime de l'application (en-tetes HTTP reels, comportement TLS, etc.).
- Les recommandations marquees "A verifier" necessitent un test manuel ou un acces a l'application en fonctionnement.
- Le projet cible n'est **jamais modifie** — l'audit est en lecture seule.
