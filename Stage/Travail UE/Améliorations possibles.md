## UI

**_Pas une priorité, à faire quand saoulé des algos_**

-  Rework toute l'UI pour faire une application plus proche de celle de Marc (jolie waybar en haut et menus contextuels)
- Une waybar centralisée avec des icônes et des textes quand on survole ces icônes
## UX

-  Possibilité de choisir entre voxel, heightmap, layer grid, implicit terrain, vraiment pertinent pour l'utilisateur final ?
## Objets environnementaux

- Se remettre sur les kelvinlets pour les objets environnementaux, pouvoir en utiliser plus d'un par objet
- Observer plus intuitivement les champs d'effets des objets environnementaux (champs)
## Génération du terrain

- Première génération plus intuitive (basée sur le snake ? Pas vraiment compris si oui pour la première génération)
## Modifications du terrain

- Permettre plusieurs brushes pour modifier le terrain et améliorer l'ergonomie
- Faire fonctionner reset terrain
- Faire fonctionner save terrain
## Performances

- Améliorer les performances lors de la modification du terrain
- Trouver et corriger le bug du clic gauche dans le vide qui fait freeze pendant 1-2 secondes
## Code

- Fix tous les warnings de compilation
- Fix toutes les erreurs au runtime (cast to BP_MissionEditorPC notamment, récurrent)
- Ré écrire les quelques interfaces bizarres
- Commenter tout mon code et commenter le code pas commenté que je comprends
## Rendu

-  Utiliser plus de matériaux différents pour le terrain
-  Changer les mesh des objets environnementaux (trop coûteux actuellement)
- Faire deux niveaux de détail : une scène pour éditer moche, et une jolie scène pour la simulation