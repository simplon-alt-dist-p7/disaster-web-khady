# Backlog du projet "Optimisation Sante+"

## USER STORIES

---

### Story 1 : Chargement initial plus rapide

**En tant que** nouvel utilisateur web,  
**je veux** que l’écran d’accueil charge en moins de 1,5 s  
**afin de** ne pas décrocher lors d’un pic de réseau lent.

- 🔴 Priorité : Haute
- 🔎 Problème d'audit corrigé : dépendances surdimensionnées (`three`, `lodash` complet, `recharts` + `victory`, `jquery`, `moment`, `bootstrap`) et ressources lourdes injectées
- 🎯 Objectif : temps de chargement < 1500 ms
- 🧱 BP associée : réduire taille des ressources / supprimer les libs inutiles / lazy-loading
- 🛠️ KPI : LCP (Lighthouse); poids du bundle JS 
- 📅 Tag roadmap : M2

---

### Story 2 : Réduction poids images

**En tant que** utilisateur récurrent,  
**je veux** que les visuels du dashboard soient plus légers  
**afin de** économiser de la data sur mon forfait.

- 🟠 Priorité : Moyenne
- 🔎 Problème d'audit corrigé :image `large.jpg` chargée en pleine résolution pour un simple fond en `opacity: 0.1`
- 🎯 Objectif : 80% des images converties en WebP
- 🧱 BP associée : compression d’images / formats modernes / dimensionnement adapté / `loading="lazy"`
- 🛠️ KPI : poids total des images < 200 Ko ; poids du dossier `/assets` < 2 Mo
- 📅 Tag roadmap : M3

---

### Story 3 : Accessibilité améliorée

**En tant que** utilisateur malvoyant,  
**je veux** que les contrastes texte/fond soient conformes AA  
**afin de** pouvoir utiliser l’app sans difficulté visuelle.

- 🟡 Priorité : Basse (non observé dans l'audit GreenIT, à traiter après les optimisations à fort impact)
- 🔎 Lien avec l'audit : non confirmé, bonne pratique RGESN conservée pour plus tard
- 🎯 Objectif : conformité AA WCAG
- 🧱 BP associée : respect contrastes (RGESN 6.3) / attributs `alt` / navigation clavier
- 🛠️ KPI : score accessibilité Lighthouse > 90
- 📅 Tag roadmap : M4

---

### Story 4 : Réduction du trafic réseau *(user story supplémentaire proposée)*

**En tant que** utilisateur sur réseau mobile,  
**je veux** que l’application n’envoie pas de requêtes inutiles en continu  
**afin de** préserver ma data, ma batterie et de réduire la charge serveur.

- 🔴 Priorité : Haute
- 🔎 Problème d'audit corrigé : polling toutes les secondes (2 × `/api/payload` + 1 × `/api/server`), `/api/payload` renvoyant ~1 Mo de données factices non mises en cache (`no-store`)
- 🎯 Objectif : diviser par 10 le trafic réseau récurrent
- 🧱 BP associée : espacer/supprimer le polling / alléger la réponse API / activer le cache (`Cache-Control`)
- 🛠️ KPI : octets transférés/min; payload `/api/payload` < 5 Ko
- 📅 Tag roadmap : M2

---

### Story 5 : Animation 3D maîtrisée *(bonus)*

**En tant que** utilisateur,  
**je veux** que l’animation décorative ne tourne pas en permanence  
**afin de** ne pas surchauffer mon CPU/GPU ni vider ma batterie inutilement.

- 🟠 Priorité : Moyenne
- 🔎 Problème d'audit corrigé : scène Three.js (20 cubes) rendue en boucle infinie via `requestAnimationFrame`, sans pause quand l'onglet est masqué
- 🎯 Objectif : 0 % d'utilisation CPU/GPU liée à l'animation quand l'onglet est inactif
- 🧱 BP associée : suspendre l'animation hors écran (`visibilitychange` / `IntersectionObserver`)
- 🛠️ KPI : usage CPU au repos proche de 0 % ; frames rendues = 0 en arrière-plan
- 📅 Tag roadmap : M3

---