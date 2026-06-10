## 4. Principaux problèmes identifiés

### 4.1 Dépendances surdimensionnées (poids du bundle)

Le `package.json` embarque de nombreuses librairies lourdes et/ou redondantes :

- `three` (~600 Ko) pour une simple décoration 3D ;
- `lodash` importé **en entier** (`import _ from 'lodash'`) pour un seul usage (`_.throttle`) ;
- `recharts` **et** `victory` (deux librairies de graphiques) ;
- `jquery`, `moment`, `bootstrap` + `popper.js` non nécessaires avec React + Tailwind.

 **Impact** : bundle JavaScript très volumineux, temps de parsing/exécution élevé.

### 4.2 Scène 3D animée en continu (CPU / GPU / batterie)

Dans `src/App.tsx`, une scène Three.js (20 cubes) est rendue via `requestAnimationFrame`
**en boucle infinie, sans pause ni arrêt quand l'onglet est masqué**.

**Impact** : consommation CPU/GPU permanente, chauffe et décharge de la batterie sur
mobile/portable, alors que ce contenu est purement décoratif.

### 4.3 Polling réseau permanent et données inutiles

- `/api/payload` renvoie un bloc de **1 Mo de données factices**
  à **chaque** appel ;
- toutes les requêtes sont en `no-store` → **aucune mise en cache** ;
- un second `setInterval` recalcule les métriques toutes les 2 s.

**Impact** : trafic réseau continu et inutile, charge serveur permanente, batterie.

### 4.4 Ressources statiques lourdes injectées à la volée

`App.tsx` injecte dynamiquement `big.css` et `big.js` depuis le backend, et affiche une
image `large.jpg` **chargée en pleine résolution** uniquement pour un fond en `opacity: 0.1`.

 **Impact** : poids transféré important pour des ressources peu ou pas utiles.

### 4.5 Absence de bonnes pratiques d'écoconception

- pas de *lazy-loading* des composants/ressources lourdes ;
- pas de formats d'image modernes (WebP/AVIF) ni de dimensionnement adapté ;
- pas de stratégie de cache navigateur (`Cache-Control`).

## 5. Synthèse

| Domaine | Constat | Gravité |
|---|---|:--:|
| Poids du bundle JS | Librairies lourdes/redondantes (three, lodash, jquery, moment, 2× charts) | 🔴 Élevée |
| CPU / GPU | Animation 3D infinie non interruptible | 🔴 Élevée |
| Réseau | Polling 1 s + payload ~1 Mo non caché | 🔴 Élevée |
| Images | `large.jpg` pleine résolution pour un fond à 10 % | 🟠 Moyenne |
| Cache | `no-store` systématique, pas de `Cache-Control` | 🟠 Moyenne |
| Accessibilité | Contrastes et `alt` images à vérifier | 🟡 À confirmer |

**Conclusion** : malgré une note EcoIndex « A » sur la mesure ponctuelle, l'application
présente de **nombreux gisements d'optimisation** (bundle, animations, réseau, images).