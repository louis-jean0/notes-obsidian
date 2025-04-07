# Segmentation par flood fill

## Principe général

La segmentation par flood fill identifie une région connectée dans une image en niveaux de gris à partir d’un point initial et d’un seuil. Elle combine un algorithme de remplissage (flood fill) et une extraction de contour.
## Étape 1 : Remplissage de la région

- **Point de départ** : On choisit un point initial (x, y) dans l’image
- **Seuil** : Une valeur (ex: 128) détermine si un pixel appartient à la région
- **Processus** :
    1. **Vérification initiale** :
        - Si la valeur du pixel initial (image->getPixel(x, y)) est ≥ seuil, on arrête (le point doit être dans la région cible)
    2. **Initialisation** :
        - On ajoute le point à une queue
        - On le marque comme visité dans une matrice (visitedPoints)
        - On l’ajoute à la liste des points de la région (filledArea)
    3. **Exploration** :
        - Tant que la queue n’est pas vide :
            - On extrait le point courant de la queue.
            - On explore ses **8 voisins** (connectivité 8 : haut, bas, gauche, droite, et diagonales).
            - Pour chaque voisin :
                - **Valide** : Dans les limites de l’image (0 ≤ x < largeur, 0 ≤ y < hauteur)
                - **Non visité** : Pas encore marqué dans visitedPoints
                - **Sous le seuil** : image->getPixel(nx, ny) < seuil
                - Si ces conditions sont remplies, on l’ajoute à la queue, on le marque comme visité, et on l’inclut dans filledArea
    4. **Arrêt** : Quand la queue est vide, la région est complète.
- **Résultat** : filledArea contient tous les points de la région connectée.

Si le point initial est supérieur au seuil, on arrête. Sinon, on met ce point dans une queue et on le marque comme étant visité, puis on explore ses 8 voisins. Si un de ses voisins est valide, n'est pas déjà visité et est inférieur au seuil, on le place dans la queue pour continuer d'explorer, et on l'ajoute à la liste des points constituant la région. Dès lors que la queue est vide, on s'arrête et la région est complétée.

## Étape 2 : Extraction du contour

- **Données utilisées** : La matrice visitedPoints générée par le remplissage.
- **Processus** :
    1. **Trouver le point de départ** :
        - On scanne l’image ligne par ligne pour trouver le premier point visité (ex: edgeStart).
        - Si aucun point n’est trouvé, erreur ("No island found").
    2. **Parcours du contour** :
        - On commence à edgeStart avec une direction initiale (droite par exemple)
        - On explore les 8 voisins dans l’ordre antihoraire à partir de la direction courante
        - Pour chaque voisin :
            - Si valide et visité :
                - On l’ajoute à contourPoints
                - On le marque comme non visité (évite les boucles infinies)
                - On tourne à **gauche** (direction + 6 modulo 8) pour suivre le bord extérieur
            - Sinon, on teste la direction suivante
        - On continue jusqu’à revenir à edgeStart ou jusqu’à ne plus trouver de voisin valide
- **Résultat** : contourPoints contient le contour dans le sens antihoraire

