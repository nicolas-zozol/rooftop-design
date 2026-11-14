Panneau de parchemin ouvert sous le cadre : les stories d'un batch, ses dépendances et l'Action à copier.

## L'appelant fournit
- le batch : id court et complet (B1, B-PLUGIN-1), état, nombre de stories, OQ ouvertes (dont RLOQ) et leur ancre ;
- chaque story : slug, dernière étape franchie, la case qui porte ses OQ ou RLOQ, la case bloquée par une dépendance et le batch attendu ;
- pour chaque dépendance qui arrête le boss : son chemin de déblocage numéroté ;
- le texte de l'Action, dont la première ligne est la commande ; l'appelant garde le texte modifié et sait le rétablir ;
- la position de la tuile cliquée, pour l'encoche.

## Quand l'utiliser
- Au clic sur une tuile, un emplacement tenu ou la pile Hors batch, sous le cadre, à `marge-panneau`. Un seul chantier ouvert par epic.
- Batch tenu : pas de stories ni d'Action, la phrase « Ce batch attend la soumission de B1. » et « Aucune action tant que B1 n'est pas soumis. »
- Hors batch : titre « Hors batch », la note jointe en premier dans la colonne de droite, pas d'Action.

## À faire
- Fond `chantier`, bord `chantier-bord`, `ombre-panneau`, `radius-panneau`, largeur au moins `taille-chantier`.
- Titres en Alegreya : `chantier-titre`, `chantier-section`, `action-titre`. Slugs en `slug`, éditeur en `editeur`.
- Grille des stories : slug, puis six cases (`.rt-case`) titrées ready, brainstorm, design file, code, submitted, done. Rien d'autre sur la ligne. OQ, RLOQ et dépendances se posent dans la case qu'elles bloquent ; fond `blocage-fond` quand on suit le chemin d'une dépendance.
- Colonne « Où le boss s'arrête » de `taille-dependances`, séparée par `chantier-filet` : une ligne par story arrêtée, une ligne cochée pour celles qui vont jusqu'au bout, puis le chemin numéroté et « Voir sur le plan ».
- Action : la première ligne est « boss atw B-PLUGIN-1 », la suite dit où le boss va s'arrêter.
- Action : « Rétablir » au contour `chantier-bord`, « Copier » plein `chantier-bouton`, éditeur `chantier-editeur`. Le bouton devient « Copié » un instant.

## À éviter
- Ne montre comme bloquante qu'une dépendance qui arrête le boss avant la fin ; pas de verrou sur les batchs suivants, les emplacements tenus le disent déjà.
- L'éditeur ne modifie rien dans le projet : il prépare le texte qu'on copie.
- Pas d'autre bouton d'action : Rooftop reste en lecture seule.
