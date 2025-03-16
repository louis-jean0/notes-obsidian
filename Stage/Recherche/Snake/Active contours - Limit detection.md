_Shree Nayar_

![[Pasted image 20250312112802.png]]

Contour actif : en entrée, on a un contour approximatif de l'objet, le but est de faire évoluer ce contour vers les contours exacts de l'objet

On déforme le contour initial pour le rapprocher des pixels à fort gradients, dans le but de le rendre lisse.

Sont très utiles pour traquer des contours dans une vidéo (déformation par le temps) ou pour plusieurs points de vues par exemple, utilisé dans Photoshop (magnetic lasso tool)

**Un contour est initialement représenté par une liste ordonnée de vertices 2D, avec chaque arête entre deux points de taille fixe égale et droite.**

![[Pasted image 20250312113818.png|400]]

On va voir l'algorithme le plus basique, glouton.

Sur l'image initiale, on calcule la norme du gradient au carré (NGC), puis on floute cette image pour avoir un meilleur contraste entre l'objet et le fond.

![[Pasted image 20250312120342.png|400]]

On veut maximiser la somme de NGC, pour déplacer les points sur le contour, ce qui revient à minimiser l'opposé de cette somme. Cette énergie s'appelle l'énergie de l'image (ou du gradient).

$E_{image} = -\Sigma^{n-1}_{i=0}\lVert \nabla n_\sigma \times I(v_i) \rVert ^2$

avec $n_\sigma$ le noyau pour le flou.

1. Pour chaque point du contour $v_i (i = 0, ..., n - 1)$, on bouge $v_i$ à une position (dans une fenêtre pré-définie W, une sorte de range) où l'énergie $E_{image}$  est minimale.
2. Si la somme des déplacements de chaque points du contour est plus petite qu'un seuil, on arrête, sinon on reprend à  l'étape 1.

Pour obtenir un rendu plus réaliste, on va utiliser une énergie interne, définie comme

$E_{contour} = \alpha E_{elastic} + \beta E_{smooth}$

$\alpha$, $\beta$ contrôlent l'influence de l'élasticité et la fluidité. Elasticité = contrôle la tendance du contour à s'étirer et à se contracter, fluidité = évite les cassures trop brutes

![[Pasted image 20250314152535.png|600]]

Avec les différences finies, on peut approximer pour chaque point $v_i$ :

![[Pasted image 20250314152855.png|600]]

Ce qui donne pour calculer les termes complets :

![[Pasted image 20250314153027.png|600]]

$E_{image}$ : mesure comment le contour s'accroche aux contours

$E_{contour}$ : mesure l'élasticité et la fluidité du contour