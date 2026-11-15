Fanion doré du drapeau de priorité. Un module en porte au plus un, posé sur une de ses epics, un de ses batchs ou une de ses stories.

## L'appelant fournit
- le module et sa cible : une epic (son INFIX), un batch (« B-PLUGIN-1 »), une story (son slug), ou rien ;
- le `title` qui nomme les deux : « Drapeau de priorité de RB Compound : la story plugin-split (PLUGIN, B1) ».

## Quand l'utiliser
- Sur la cible même : dans l'en-tête d'epic, à gauche de l'anneau ; à droite de l'id d'une tuile ou d'un emplacement tenu ; en haut à droite d'une mini-tuile, en 11 px ; devant le slug dans le chantier.
- Sur le batch qui contient la story au drapeau : sa tuile et sa mini-tuile le portent aussi, pour qu'on le trouve epic repliée.
- Dans le bandeau du module, la puce `.rt-puce-drapeau` nomme la cible en mono. Un module de plusieurs epics sans drapeau écrit « aucun drapeau » (`.rt-puce-drapeau--aucun`) ; une epic seule ne l'écrit qu'en modes Déplacer et Drapeau.

## À faire
- Triangle `priorite` sur hampe `priorite-bord`, 13 px (11 px sur une mini-tuile), `role="img"`, `aria-label="drapeau de priorité"`.
- Le poser ailleurs dans le même module le déplace ; recliquer sur la cible le retire.

## À éviter
- Ne le confonds pas avec le drapeau rectangulaire vert de « fait », ni avec l'icône d'epic au trait.
- Jamais deux drapeaux dans un module, jamais un drapeau sur un module entier : la priorité d'un module, c'est sa place sur le plan.
