**_Marc** Hartley_

**Chapitre 4 de la thèse de Marc Hartley**

![[Thesis-65-92.pdf]]

1. **Dans quel contexte ce travail s’inscrit-il ?

Génération procédurale de terrains, avec un accent particulier sur la création de paysages réalistes et adaptables, tant terrestres que sous-marins (ex. : îles de récifs coralliens, canyons). Vise à répondre aux besoins des utilisateurs (artistes, scientifiques, roboticiens) en combinant des approches multi-échelles, l’interaction utilisateur et l’intégration de connaissances expertes. Simulation d’écosystèmes, planification robotique et visualisation graphique.

2. **Quels sont les travaux précédents celui-ci ?

Simulation d'écosystèmes : modèles basés sur une représentation de la population étudiée continue (réaction-diffusion), modèles basés sur chaque individu (Individual Based Models), modèles de champs écologiques, objets environnementaux (cf article Grosbellet et al. 2016) -> base théorique de ce travail

3. **Quel est le problème soulevé par l’article ?


Est-il possible de fournir un moyen d’interaction pour la génération de terrains permettant à l’utilisateur d’interagir avec toutes les features à toutes les échelles en temps réel dans des environnements terrestres et sous-marins ?


4. **Pourquoi le problème soulevé par l’article est-il important ?

Il répond à un besoin pratique dans des domaines comme la robotique (génération de terrains pour simulations), la biologie (étude des écosystèmes sous-marins) et les ICC plus globalement (ex. : création de paysages réalistes).

5. **Pourquoi le problème soulevé par l’article est-il difficile à résoudre ou n’a pas été résolu ?

Gérer des structures de tailles très différentes (rocher vs montagne) nécessite une représentation cohérente à toutes les échelles, ce qui est coûteux.
Permettre des modifications dynamiques sans compromettre le réalisme de la scène exige un équilibre entre contrôle manuel et automatisation.
Intégrer des connaissances expertes pour simuler des évolutions réalistes est compliqué.
Les méthodes traditionnelles sont statiques ou ne permettent pas une interaction fine, et les simulations physiques sont trop lentes pour une utilisation interactive.

6. **Comment le problème est-il résolu ?

Initialisation avec un champ de hauteur et un niveau d'eau, initialisation des objets environnementaux (peuvent être définis par l'utilisateur, sinon par défaut), initialisation des différents matériaux avec leurs critères (vitesse de diffusion, masse, taux de déclin, influence des courants). Ensuite, processus de génération : première phase : instanciation -> à chaque itération, de nouveaux objets environnementaux sont créés et placés dans les endroits les plus probables, en utilisant des fonctions de fitness et de skeleton fitting par rapport aux champs scalaires (température, humidité, élévation). Puis mise à jour de l'environnement -> les attributs environnementaux sont mis à jour par chaque objet environnemental, qui déposent et absorbent les matériaux tout en modifiant les courants et le champ de hauteur. Enfin, les courants et la pente du terrain déplacent les matériaux du terrain jusqu'à trouver un équilibre. Pendant la génération, l'utilisateur peut perturber le processus de génération en altérant la distribution et les formes des objets environnementaux. (Les objets environnementaux peuvent être des arbres, des coraux, des rochers, des rivières).

7. **Que peut-on dire des résultats, principalement en terme de qualité et en terme de performances ?

Les résultats sont plausibles et flexibles, capables de générer des scènes variées (îles, canyons, colonies de coraux) à différentes échelles. La qualité biologique est correcte car les paramètres ont été choisis en concordance avec des experts du milieu, la qualité visuelle repose sur les fonctions de fitness choisies.

8. **Quelles sont les limites des travaux présentés dans cet article ?

Champs scalaires 2D, excluant les structures 3D complexes comme les cavités ou l'intérieur des structures de corail. La qualité dépend fortement de la collaboration avec des experts pour définir des fonctions réalistes. A voir les performances de la méthode avec un grand nombre d’objets environnementaux.