_Francois Grosbellet, Adrien Peytavie, Eric Guérin, Eric Galin, Stéphane Mérillou and Bedrich Benes_

![[2016-environmental-objects.pdf]]

1. **Dans quel contexte ce travail s’inscrit-il ?**

Modélisation de larges scènes comportants beaucoup d'objets interagissant entre eux et avec leur environnement.

2. **Quels sont les travaux précédents celui-ci ?**

Les travaux précédents celui-ci ont mis au point des techniques de modélisation procédurale de bâtiments et d'environnements naturels, de simulation de phénomènes naturels, et des algorithmes d'érosion, vieillissement et altération.

3. **Quel est le problème soulevé par l’article ?**

Traditionnellement, les changements au sein d'une scène sont traduits par l'ajout de détails à chaque objet dans une scène statique pré-générée ou en effectuant une simulation globale, ce qui demande soit une édition manuelle complexe, soit une grande puissance de calcul.

4. **Pourquoi le problème soulevé par l’article est-il important ?**

Les approches précédemment citées ne sont pas compatibles avec un pipeline de travail industriel, où les artistes et les designers ont besoin de feedback interactif, rapide et intuitif pour contrôler les effets.

5. **Pourquoi le problème soulevé par l’article est-il difficile à résoudre ou n’a pas été résolu ?**

Les interactions locales sont coûteuses via des simulations, il y a un aspect de contrôle de l'utilisateur et de modification post élévation qui est difficile à équilibrer avec la génération procédurale, et les méthodes existantes peinent à conserver un haut niveau de détail dès lors que l'on agrandit la scène (scalabilité).

6. **Comment le problème est-il résolu ?**

Les auteurs utilisent une approche plus locale, en partant de l'observation que les objets influencent les autres objets et l'environnement dans un rayon limité. Plutôt que d'appliquer une simulation globale, ils définissent des objets environnementaux : une simple extension de la géométrie, caractérisés par un ensemble de champs scalaires et d'effets procéduraux. Les champs scalaires définissent l'impact de l'objet sur l'environnement, et les effets procéduraux déterminent comment les objets réagissent aux changements de l'environnement, en ajoutant de la géométrie dans la scène. Un autre aspect fondamental de cette méthode est l'organisation des objets de la scène en hiérarchie, évitant de recalculer la scène en permanence, permettant même l'instanciation de groupes d'objets.

7. **Que peut-on dire des résultats, principalement en terme de qualité et en terme de perfor-
mances ?**

Les résultats sont cohérents et réalistes. Il est possible de générer une scène en quelques secondes, vs plusieurs minutes pour les méthodes classiques. Détails de l'ordre du centimètre.

8. **Quelles sont les limites des travaux présentés dans cet article ?**

Absence de réalité physique : les feuilles ne bougent pas avec le vent, accumulation compliquée : pas possible de mettre de la neige sur un tas de feuille par exemple, et scène statique : pas de changement avec le temps.