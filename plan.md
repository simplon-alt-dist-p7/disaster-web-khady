# Plan d'action - Optimisation écoconception

> Plan construit à partir de l'analyse des problèmes (`audit-initial.md`) et du backlog (`backlog.md`).
> Chaque action corrige un problème **mesuré** et vise un **gain mesurable**.

## 1. User stories sélectionnées

| Ordre | Story | Priorité | Problème ciblé |
|:--:|---|:--:|---|
| 1 | Story 1 - Chargement initial plus rapide | 🔴 Haute | Dépendances surdimensionnées (bundle) |
| 2 | Story 4 - Réduction du trafic réseau | 🔴 Haute | Polling permanent + payload 1 Mo non caché |
| 3 | Story 2 - Réduction poids images | 🟠 Moyenne | `large.jpg` pleine résolution |
| 4 | Story 5 - Animation 3D maîtrisée | 🟠 Moyenne | Boucle 3D infinie (CPU/GPU) |
| 5 | Story 3 - Accessibilité | 🟡 Basse | Reportée (non observée dans l'audit) |

## 2. Modifications prévues

### Story 1 - Chargement initial plus rapide
- Remplacer `import _ from 'lodash'` par un import ciblé (`lodash.throttle` ou natif).
- Supprimer les dépendances inutiles : `jquery`, `moment`, `victory` (doublon de `recharts`), `bootstrap`/`popper.js`.
- Charger `three` en *lazy-loading* (import dynamique) ou retirer la décoration 3D.
- Supprimer l'injection de `big.js` / `big.css`.
- **Justification** : bundle JS très volumineux = temps de parsing/exécution élevé.

### Story 4 - Réduction du trafic réseau
- Espacer le polling (1 s à 30 s) ou le déclencher seulement quand l'onglet est actif (à voir).
- Alléger `/api/payload` (supprimer le bloc de 1 Mo de données factices).
- Ajouter une stratégie de cache (`Cache-Control`) sur les ressources statiques.
- Supprimer le `setInterval` de recalcul redondant des métriques.
- **Justification** : trafic réseau continu et inutile, charge serveur, batterie.

### Story 2 - Réduction poids images
- Convertir `large.jpg` en WebP/AVIF et la redimensionner à l'usage réel (fond à 10 %).
- Ajouter `loading="lazy"` sur les images non critiques.
- **Justification** : image pleine résolution pour un simple fond.

### Story 5 - Animation 3D maîtrisée
- Suspendre l'animation quand l'onglet est masqué (`visibilitychange`) ou hors écran (`IntersectionObserver`).
- Réduire le nombre d'objets / la fréquence de rendu si conservée.
- **Justification** : boucle `requestAnimationFrame` infinie -> CPU/GPU permanents.

## 3. Ordre de réalisation

1. **Story 1 (bundle)** - plus gros levier, à traiter en premier.
2. **Story 4 (réseau)** - fort impact, indépendant du reste.
3. **Story 2 (images)** - gain complémentaire rapide.
4. **Story 5 (animation 3D)** - finalisation.
5. **Story 3 (accessibilité)** si le temps le permet.

> Règle : un commit par modification + vérification que l'application fonctionne toujours après chaque étape.

## 4. Résultats attendus

| Indicateur | Avant (audit initial) | Cible après |
|---|---:|---:|
| Poids du bundle JS | élevé (libs lourdes) | < 300 Ko (gzip) |
| Trafic réseau récurrent | 1 Mo × 2 / s | divisé par 10 |
| Payload `/api/payload` | 1 Mo | < 5 Ko |
| Poids des images | `large.jpg` pleine résolution | < 200 Ko |
| CPU au repos (onglet masqué) | animation continue | ≈ 0 % |
| Temps de chargement | baseline | < 1500 ms |
| EcoIndex / note | A (mesure ponctuelle) | A maintenu, gains réels en usage |

**Objectif global** : conserver le fonctionnement de l'application (dashboard de métriques)
tout en réduisant nettement le poids transféré, la charge CPU/réseau et la consommation
énergétique, avec des gains chiffrés et reproductibles à présenter dans `audit-final.md`.
