Cube de bois qui dit l'étape d'une story : une couleur par étape atteinte, une coche ronde quand elle est terminée.

## L'appelant fournit
- l'étape de la story : 0 à écrire, 1 écrite, 1,5 design en brouillon, 2 design approuvé, 3 code fini, 4 soumise, 5 terminée.

## Quand l'utiliser
- Sur une tuile : un cube par story, l'étape la plus avancée seulement (`taille-cube`).
- Dans le chantier : une case par étape (`chantier-case`), chaque étape franchie a son cube (`taille-cube-chantier`), l'étape en cours à moitié en tirets.

## À faire
- `etape-ecrite`, `etape-design`, `etape-code`, `etape-soumise`, relief `ombre-cube`, rayon `radius-cube`. Terminée : `etape-terminee`, `radius-rond`, coche `surface`.
- À écrire : cube vide en tirets `etape-a-ecrire`. À moitié : tirets de la couleur de l'étape.
- Donne au groupe de cubes un `aria-label` (« avancement des 4 stories »).

## À éviter
- Ne laisse pas la couleur seule dire l'étape : le chantier l'écrit en toutes lettres.
- Pas d'autre forme que le cube et la coche.
