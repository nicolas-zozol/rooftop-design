Panneau de parchemin ouvert sous le cadre : les stories d'un batch, ses dépendances et l'Action à copier.

## L'appelant fournit
- le batch : id court et complet (B1, B-PLUGIN-1), état, nombre de stories, OQ ouvertes et leur ancre ;
- chaque story : slug, étape, et l'id du batch qu'elle attend si elle est bloquée ;
- les dépendances : verrous sur les batchs suivants, blocages venant d'autres epics et leur chemin de déblocage numéroté ;
- le texte de l'Action, dont la première ligne est la commande ; l'appelant garde le texte modifié et sait le rétablir ;
- la position de la tuile cliquée, pour l'encoche.

## Quand l'utiliser
- Au clic sur une tuile, un emplacement tenu ou la pile Hors batch, sous le cadre, à `marge-panneau`. Un seul chantier ouvert par epic.
- Batch tenu : pas de stories ni d'Action, la phrase « Ce batch attend la soumission de B1. » et « Aucune action tant que B1 n'est pas soumis. »
- Hors batch : titre « Hors batch », la note jointe en premier dans la colonne de droite, pas d'Action.

## À faire
- Fond `chantier`, bord `chantier-bord`, `ombre-panneau`, `radius-panneau`, largeur au moins `taille-chantier`.
- Titres en Alegreya : `chantier-titre`, `chantier-section`, `action-titre`. Slugs en `slug`, éditeur en `editeur`.
- Grille des stories : slug, cinq cases (`.rt-case`), « où elle en est ». Story bloquée : statut `blocage-texte` au cercle barré ; fond `blocage-fond` quand on suit son chemin.
- Colonne de `taille-dependances` séparée par `chantier-filet`, puis « Voir sur le plan » qui affiche le bandeau du chemin.
- Action : « Rétablir » au contour `chantier-bord`, « Copier » plein `chantier-bouton`, éditeur `chantier-editeur`. Le bouton devient « Copié » un instant.

## À éviter
- L'éditeur ne modifie rien dans le projet : il prépare le texte qu'on copie.
- Pas d'autre bouton d'action : Rooftop reste en lecture seule.
