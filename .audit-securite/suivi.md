## 2. Liste des recommandations — ANNX-3 (Sécurité côté navigateur)

| N° | Intitulé | Dossier | Statut |
| --- | --- | --- | --- |
| R1 | Mettre en œuvre TLS à l'état de l'art | 01-TLS-et-transport-securise | |
| R2 | Mettre en œuvre HSTS | 01-TLS-et-transport-securise | |
| R3 | Surveiller les CT logs | 01-TLS-et-transport-securise | |
| R4 | Utiliser l'API DOM à bon escient | 02-Composition-des-pages-et-DOM | |
| R5 | Dissocier clairement la composition des pages web | 02-Composition-des-pages-et-DOM | |
| R6 | Expliciter la nature d'une ressource avec l'en-tête Content-Type | 02-Composition-des-pages-et-DOM | |
| R7 | Vérifier l'échappement des contenus inclus | 03-Echappement-et-validation-des-contenus | |
| R8 | Vérifier la conformité des données issues de sources externes | 03-Echappement-et-validation-des-contenus | |
| R9 | Proscrire l'usage de la fonction eval() | 03-Echappement-et-validation-des-contenus | |
| R10 | Proscrire l'usage de constructions basées sur l'évaluation de code | 03-Echappement-et-validation-des-contenus | |
| R11 | Contrôler l'intégrité des contenus internes | 04-Integrite-des-ressources | |
| R12 | Contrôler l'intégrité des contenus tiers | 04-Integrite-des-ressources | |
| R13 | Restreindre les contenus aux ressources fiables | 04-Integrite-des-ressources | |
| R14 | Mettre en œuvre CSP par en-tête HTTP | 05-Content-Security-Policy-CSP | |
| R14- | Mettre en œuvre CSP par balise meta dans les pages HTML | 05-Content-Security-Policy-CSP | |
| R15 | Interdire des contenus inline | 05-Content-Security-Policy-CSP | |
| R16 | Définir la directive default-src | 05-Content-Security-Policy-CSP | |
| R17 | Utiliser CSP contre le clickjacking | 05-Content-Security-Policy-CSP | |
| R18 | Utiliser X-Frame-Options contre le clickjacking | 05-Content-Security-Policy-CSP | |
| R19 | Étudier les risques liés à la collecte de rapports CSP | 05-Content-Security-Policy-CSP | |
| R20 | Réduire l'impact des requêtes silencieuses via CSP | 05-Content-Security-Policy-CSP | |
| R21 | Définir la stratégie de construction de l'en-tête Referer | 06-Politique-Referer | |
| R22 | Modifier ponctuellement l'en-tête Referer | 06-Politique-Referer | |
| R23 | Ne pas stocker des informations sensibles dans les bases de données locales | 07-Stockage-local | |
| R23- | Éviter de stocker des informations sensibles dans les bases de données locales | 07-Stockage-local | |
| R24 | Ne pas stocker des informations sensibles dans les bases de données IndexedDB | 07-Stockage-local | |
| R24- | Éviter de stocker des informations sensibles dans les bases de données IndexedDB | 07-Stockage-local | |
| R25 | Proscrire l'usage de l'API Web SQL Database | 07-Stockage-local | |
| R26 | Ne pas stocker d'informations sensibles dans les cookies | 08-Cookies | |
| R27 | Cloisonner les sessions au moyen de noms de domaine distincts | 08-Cookies | |
| R28 | Définir le path d'un cookie | 08-Cookies | |
| R29 | Maîtriser l'accès aux cookies en JavaScript | 08-Cookies | |
| R30 | Proscrire l'accès en JavaScript à un cookie de session | 08-Cookies | |
| R31 | Limiter le transit des cookies aux flux sécurisés | 08-Cookies | |
| R32 | Définir une stratégie stricte d'envoi des cookies en cross-site | 08-Cookies | |
| R33 | Définir une stratégie stricte d'envoi des cookies de session en cross-site | 08-Cookies | |
| R34 | Encoder les réponses XMLHttpRequest | 09-XMLHttpRequest-XHR-et-Fetch | |
| R35 | Choisir une API selon sa méthode HTTP | 09-XMLHttpRequest-XHR-et-Fetch | |
| R36- | Utiliser XHR avec la méthode GET sous certaines conditions | 09-XMLHttpRequest-XHR-et-Fetch | |
| R36 | Utiliser XHR avec la méthode POST | 09-XMLHttpRequest-XHR-et-Fetch | |
| R36+ | Utiliser XHR avec la méthode PUT | 09-XMLHttpRequest-XHR-et-Fetch | |
| R37 | Compléter la mise en œuvre de XHR par une configuration CSP | 09-XMLHttpRequest-XHR-et-Fetch | |
| R38 | Protéger les appels XHR par un contrôle anti-CSRF | 09-XMLHttpRequest-XHR-et-Fetch | |
| R39 | Mettre en œuvre un preflight lors des appels CORS | 10-CORS | |
| R40 | Vérifier la valeur de l'Origin lors de la réception d'une requête CORS | 10-CORS | |
| R41 | Cloisonner les services web au moyen de noms de domaines distincts | 11-Isolation-des-services | |
| R42 | Éviter l'usage de bibliothèques publiques effectuant des appels CORS | 11-Isolation-des-services | |
| R42- | Isoler l'utilisation de bibliothèques publiques effectuant des appels CORS | 11-Isolation-des-services | |
| R43 | Anonymiser le chargement des ressources en cross-origin | 11-Isolation-des-services | |
| R44 | Préférer l'utilisation de l'API Fetch à XMLHttpRequest | 12-API-Fetch | |
| R45 | Sécuriser l'ouverture de nouvelles fenêtres | 13-Fenetres-et-navigation | |
| R46 | Définir une stratégie d'ouverture en cross-origin | 13-Fenetres-et-navigation | |
| R47 | Utiliser le mode strict | 14-Mode-strict-JavaScript | |
| R48 | Isoler les traitements par Web Workers | 15-Web-Workers | |
| R48+ | Isoler les traitements par Web Worker et Origin « data : » | 15-Web-Workers | |
| R49 | Formaliser les échanges en utilisant l'API de Message | 16-API-de-Message-postMessage | |
| R50 | Cloisonner les traitements dans des iframes | 17-Iframes-et-sandboxing | |
| R51 | Cloisonner les traitements avec une sandbox | 17-Iframes-et-sandboxing | |
| R52 | Favoriser la déclaration de sandbox via CSP | 17-Iframes-et-sandboxing | |
| R52+ | Cloisonner les traitements par une iframe sur une seconde Origin | 17-Iframes-et-sandboxing | |
| R53 | Formaliser les échanges en utilisant l'API de Message | 18-Communication-inter-contextes | |
| R54 | Définir l'Origin lors de l'utilisation de l'API de Message | 18-Communication-inter-contextes | |
| R55 | Contrôler l'Origin lors de l'utilisation de l'API de Message | 18-Communication-inter-contextes | |
| R56 | Compléter la déclaration d'une API de messages par la définition d'une CSP | 18-Communication-inter-contextes | |
| R57 | Proscrire l'écriture de document.domain | 19-Pratiques-obsoletes-et-dangereuses | |
| R58 | Proscrire l'usage de JSON-P | 19-Pratiques-obsoletes-et-dangereuses | |
| R59 | Définir des profils de déploiement spécifiques aux contextes | 20-Deploiement-et-configuration | |
| R60 | Empêcher le déploiement d'un profil non adapté au contexte | 20-Deploiement-et-configuration | |
| R61 | Limiter les composants logiciels tiers | 21-Composants-logiciels-tiers | |
| R62 | Maintenir à jour les composants logiciels tiers utilisés | 21-Composants-logiciels-tiers | |
| R63 | Ne pas modifier le cœur des composants logiciels tiers utilisés | 21-Composants-logiciels-tiers | |

---

## Synthèse

| Catégorie | Total | Conforme | Partiel | Non conforme | À vérifier |
|-----------|-------|----------|---------|--------------|------------|
| TLS et transport | 3 | | | | |
| DOM et composition | 3 | | | | |
| Échappement/validation | 4 | | | | |
| Intégrité des ressources | 3 | | | | |
| CSP | 8 | | | | |
| Politique Referer | 2 | | | | |
| Stockage local | 4 | | | | |
| Cookies | 8 | | | | |
| XHR et Fetch | 5 | | | | |
| CORS | 2 | | | | |
| Isolation des services | 3 | | | | |
| API Fetch | 1 | | | | |
| Fenêtres/navigation | 2 | | | | |
| Mode strict | 1 | | | | |
| Web Workers | 2 | | | | |
| API de Message | 2 | | | | |
| Iframes/sandboxing | 3 | | | | |
| Communication inter-contextes | 4 | | | | |
| Pratiques obsolètes | 2 | | | | |
| Déploiement | 2 | | | | |
| Composants tiers | 3 | | | | |
| **TOTAL** | **63** | | | | |
