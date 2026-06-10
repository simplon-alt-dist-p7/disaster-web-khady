# Audit GreenIT initial

## 1. Contexte de la mesure

- **Date de la mesure** : 10/06/2026 
- **Outil** : GreenIT-Analysis
- **Application** : frontend `http://localhost:3000` + backend `http://localhost:5001`

## 2. Résultats de l'audit (EcoIndex)

| Indicateur | Valeur mesurée |
|---|---:|
| Nombre de requêtes | **14** |
| Taille de la page (Ko) | **11** |
| Taille du DOM | **140** éléments |
| GES (gCO2e) | **1,22** |
| Eau (cl) | **1,83** |
| EcoIndex | **88,97** |
| Note | **A** |

### Capture d'écran justificative

![Résultats GreenIT-Analysis](./Capture%20d%E2%80%99e%CC%81cran%202026-06-10%20120749.png)

## 3. Lecture critique des résultats

La note EcoIndex affichée est **A (88,97)**, ce qui semble excellent. Toutefois cette note
**ne reflète pas la consommation réelle de l'application** pour deux raisons :

1. **La mesure a porté sur une seule URL ponctuelle** (`/api/payload?ts=…`), c'est-à-dire
   une réponse JSON isolée, et non sur le parcours complet de l'interface.
2. **L'EcoIndex est une mesure instantanée du chargement initial.** Il ne capte donc pas le
   comportement de l'application **dans la durée** (animations, polling réseau permanent),
   qui est précisément l'endroit où cette application est la plus coûteuse.

