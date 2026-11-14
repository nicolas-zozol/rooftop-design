Bandeau noir qui coiffe un module sur le plan : surtitre MODULE, nom, avancement, barre de 4 px.

## L'appelant fournit
- le nom du module (« RB Compound », « Toolman »), tel qu'écrit dans le projet ;
- l'avancement du module : la moyenne de l'avancement de ses epics, en pourcentage entier, écrit « 27 % » et reporté sur la barre ;
- l'état : le glyphe demi-plein `module-en-cours` seulement si le module est en cours.

## Quand l'utiliser
- Au-dessus de chaque cadre de module (`.rt-module`), hauteur `taille-en-tete`.
- Fusionné en haut de la carte d'une epic quand elle est seule dans son module : un seul en-tête noir, pas deux.

## À faire
- Fond `bandeau`, nom en `bandeau-texte` (style `module`), surtitre et icône grille en `bandeau-muet` (style `surtitre`).
- Barre : piste `bandeau-piste`, plein `bandeau-texte`, hauteur `taille-barre-module`.

## À éviter
- Pas de couleur de marque dans le bandeau : l'ambre `module-en-cours` ne sert qu'au glyphe.
- Pas de deuxième ligne : le titre long d'un module s'arrête par une ellipse.
