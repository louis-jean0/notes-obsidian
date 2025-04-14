### 1. Contexte et objectif

- **Qu’est-ce que les kelvinlets :** solutions analytiques pour modéliser des déformations élastiques en temps réel, basées sur la théorie de l’élasticité linéaire. Permettent de simuler des déformations réalistes (comme si un matériau était poussé, étiré ou tordu) dans des environnements virtuels, comme des jeux ou des applis 3D.
- **Problème résolu** :  les méthodes traditionnelles (ex. : éléments finis) sont trop lentes pour des applis interactives. Les kelvinlets offrent une alternative rapide avec des résultats plausibles physiquement.
- **Application dans mon stage** :  les kelvinlets pourraient être utilisés pour simuler la vorticité ou les interactions avec des objets environnementaux (ex. : rochers, îles), actuellement 1 kelvinlet par objet, possibilité d'en mettre plusieurs et de les stocker dans le json final aussi
### 2. Comment ça marche ?

- **Idée principale** :  un kelvinlet est une déformation locale causée par une force appliquée. Comme un brush qui déforme un matériau élastique en suivant les lois de la physique.
- **Types de pinceaux** :
    - **Grab** : déplace une zone comme si on la tirait.
    - **Scale** : étire ou compresse une région.
    - **Twist** : tord une zone (utile pour la vorticité ?).
    - **Pinch** : pince ou creuse une région.
- **Régularisation** :  les kelvinlets classiques divergent à l’infini près du point d’application. Les "regularized kelvinlets" utilisent un paramètre pour lisser la déformation.
- **Maths simplifiées** :  les déformations sont calculées avec des formules fermées (pas d’itérations lourdes), ce qui garantit une rapidité adaptée au temps réel.
### 3. Avantages

- **Rapidité**,  **réalisme**  (les déformations respectent les propriétés élastiques d’un matériau)
- **Flexibilité** : on peut combiner plusieurs kelvinlets (ex. : plusieurs forces sur un même objet) pour des effets complexes..
- **Contrôle** : on peut ajouter des contraintes (ex. : fixer certains points)
### 4. Limites

- **Paramètres** : il faut tuner la régularisation pour éviter des déformations bizarres près des bords.
- **Complexité des combos** : trop de kelvinlets peut devenir dur à contrôler.