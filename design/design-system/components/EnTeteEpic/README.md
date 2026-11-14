En-tête d'une carte d'epic : drapeau, étiquette INFIX, titre, anneau d'avancement, pourcentage, chevron.

## L'appelant fournit
- l'INFIX en capitales (PLUGIN, TOOLMAN) et le titre de l'epic ;
- l'avancement en pourcentage et son état : à faire, en cours, fini, fait ;
- l'état ouvert ou replié, qui choisit le chevron et `aria-expanded` ;
- le fait que l'epic est bloquée, qui passe la carte en tirets.

## Quand l'utiliser
- En haut de chaque carte d'epic (`.rt-epic`), déroulée ou repliée, hauteur `taille-en-tete`, filet `ligne` dessous.

## À faire
- Anneau sur piste `ligne-forte` : arc `avancement-a-faire`, `avancement-en-cours`, `avancement-fini` (coche dans l'anneau à 100 %) ou `avancement-fait`.
- INFIX en style `infix` sur `etiquette-infix` ; titre en `titre-epic`, coupé par une ellipse.
- Epic bloquée : bordure en tirets `tirets-bloquee`.

## À éviter
- Pas de pourcentage sans anneau, pas d'anneau sans pourcentage.
- Le chevron est le seul bouton de l'en-tête.
