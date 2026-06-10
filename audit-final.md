#### Résultats GreenIT après modifications (US1)

- **Date de la mesure** : 10/06/2026 15:37:54
- **Outil** : GreenIT-Analysis
- **URL mesurée** : `http://localhost:5001/api/payload?ts=1781098643105`


| Indicateur             | Avant (audit initial) | Après (US1) | Évolution    |
| ---------------------- | --------------------- | ----------- | ------------ |
| Nombre de requêtes     | 14                    | **7**       | **−50 %**    |
| Taille de la page (Ko) | 11                    | **5**       | **−55 %**    |
| Taille du DOM          | 140                   | 140         |              |
| GES (gCO2e)            | 1,22                  | **1,20**    | **−2 %**     |
| Eau (cl)               | 1,83                  | **1,80**    | **−2 %**     |
| EcoIndex               | 88,97                 | **89,90**   | **+0,93 pt** |
| Note                   | A                     | **A**       |              |


Résultats GreenIT après US1

**Lecture** : la suppression de Bootstrap CDN, des polices superflues, de `lodash` et des
dépendances inutilisées réduit le nombre de requêtes (−7) et le poids transféré (−6 Ko).
L'EcoIndex progresse légèrement ; le DOM reste identique car la structure HTML n'a pas changé.