# Journal des optimisations

> Suivi de toutes les modifications d'écoconception réalisées sur l'application,
> avec la **justification** de chaque choix et le **problème d'audit** corrigé.
> Référence des problèmes : voir `audit-initial.md`

## Légende

- 🔎 **Problème ciblé** : le problème mesuré/observé que la modification corrige
- 🛠️ **Modification** : ce qui a été changé concrètement
- 💡 **Pourquoi ce choix** : justification (gain attendu, alternative écartée)
- ⚖️ **Impact fonctionnel** : ce qui change (ou non) pour l'utilisateur

---

## US1 — Chargement initial plus rapide (réduction du bundle)

Problèmes d'audit visés : (dépendances surdimensionnées) et (ressources lourdes).

### 1.1 Suppression de la librairie `lodash` (`src/App.tsx`)

- 🔎 **Problème ciblé** : `import _ from 'lodash'` chargeait la **librairie entière** (70 Ko) pour un seul usage (`_.throttle`).
- 🛠️ **Modification** : remplacement par une petite fonction `throttle` native ( 15 lignes) écrite dans le fichier.
- 💡 **Pourquoi ce choix** : éviter d'embarquer une dépendance complète pour une seule fonction. Une implémentation native couvre exactement le besoin (limiter le `resize`) sans coût de bundle.
- ⚖️ **Impact fonctionnel** : aucun — le redimensionnement de la scène 3D fonctionne à l'identique.

### 1.2 Nettoyage de la feuille de styles (`src/index.css`)

- 🔎 **Problème ciblé** : CSS gonflé, ressources réseau bloquantes inutiles.
- 🛠️ **Modifications** :
  - suppression de l'import **Bootstrap via CDN** (`@import .../bootstrap.min.css`) ;
  - réduction de **6 polices Google à 1 seule** (Inter) ;
  - suppression de toutes les classes inutiles (`unused-class-*`, `bloat-*`, `mega-bloat-*`, `ultra-bloat-*`) ;
  - suppression des règles `.container` **dupliquées** et des keyframes gaspilleuses (`waste-cpu`, `rainbow-spin`, `mega-waste`, `.cpu-waster`, etc.).
  - le fichier passe de **187 à 22 lignes**.
- 💡 **Pourquoi ce choix** :
  - Bootstrap est **inutile** : la mise en page utilise déjà Tailwind. Le charger en plus dupliquait des styles et ajoutait une requête réseau lourde et bloquante.
  - Chaque `@import` de police est une requête bloquante : ne garder qu'Inter (réellement utilisée par le design) réduit le nombre de requêtes et le temps de rendu.
  - Les classes `bloat`/`unused` n'étaient référencées nulle part : du poids mort pur.
- ⚖️ **Impact fonctionnel** : aucun visuellement — la police principale et le fond dégradé sont conservés.

### 1.3 Retrait des dépendances inutilisées (`package.json`)

- 🔎 **Problème ciblé** : nombreuses librairies déclarées mais jamais importées dans le code.
- 🛠️ **Modification** : suppression de `axios`, `bootstrap`, `jquery`, `lodash-es`, `moment`, `morgan`, `popper.js`, `recharts`, `victory`, ainsi que `@types/jquery` et `@types/lodash-es`.
- 💡 **Pourquoi ce choix** : vérification par recherche dans tout le code (`src/` et `backend/`) qu'aucune n'était importée. `recharts` **et** `victory` faisaient en plus doublon (deux librairies de graphiques). Moins de dépendances = installation plus légère, surface de maintenance/sécurité réduite, et bundle final plus petit.
- ⚖️ **Impact fonctionnel** : aucun — ces librairies n'étaient pas utilisées.
- 📝 **Note** : `three` est **conservé** car il fait fonctionner la visualisation 3D (fonctionnalité à préserver). Son optimisation est planifiée en **US5** (suspendre l'animation hors écran).

### Gain attendu US1

- Bundle JavaScript nettement réduit (suppression de lodash + libs mortes).
- Moins de requêtes réseau bloquantes (Bootstrap CDN + 5 polices supprimés).
- Cible : **bundle JS < 300 Ko (gzip)** et **LCP < 1,5 s** (à confirmer en audit final).

---

