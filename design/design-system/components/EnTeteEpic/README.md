En-tête d'une carte d'epic : drapeau, étiquette INFIX, titre, anneau d'avancement, pourcentage, chevron.

## L'appelant fournit
- l'INFIX en capitales (PLUGIN, TOOLMAN) et le titre de l'epic ;
- l'avancement, calculé et jamais saisi : chaque story vaut ses étapes franchies sur six, un batch la moyenne de ses stories (tenu 0 %, fait 100 %), l'epic la moyenne de ses batchs ; l'état suit : 0 % à faire, en cours, 100 % fini, fait une fois mergée ;
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
