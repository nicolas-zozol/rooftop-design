Cube de bois qui dit jusqu'où une story est allée : une couleur par étape (gris, bleu clair, bleu vif, mauve, vert pour submitted), puis la coche verte ronde pour done.

## L'appelant fournit
- la dernière étape franchie par la story : pas ready, ready, brainstorm, design file, code, submitted, done ;
- dans le chantier, en plus : la case qui porte des OQ ou des RLOQ et leur nombre, la case bloquée par une dépendance et le batch attendu.

## Quand l'utiliser
- Sur une tuile : un cube par story, à la couleur de sa dernière étape franchie (`taille-cube`).
- Dans le chantier : une case par étape (`chantier-case`), un cube dans chaque étape franchie (`taille-cube-chantier`).

## À faire
- Progression : `etape-ready` (gris clair), `etape-brainstorm` (bleu clair), `etape-design-file` (bleu vif), `etape-code` (mauve), `etape-submitted` (vert, comme done), relief `ombre-cube`, rayon `radius-cube`. Done : `etape-done`, `radius-rond`, coche `surface`.
- Pas ready (des OQ ou des RLOQ sur ready) : sur la tuile, cube vide en tirets `etape-pas-ready` ; dans le chantier, la puce posée dans la case ready.
- Case bloquée : puce « 2 OQ » (le boss y répond et continue), puce pleine « 1 RLOQ » (le boss s'arrête), cercle barré `blocage` (dépendance, le boss s'arrête).
- Après le premier arrêt, cases à `opacite-hors-course` (`.rt-case--hors-course`).
- Donne au groupe de cubes d'une tuile un `aria-label` (« avancement des 4 stories ») et un `title` à chaque case du chantier.

## À éviter
- Pas d'orange ni de rouge dans la progression : ils sont réservés aux OQ et aux blocages.
- Le cube submitted a le vert de done mais reste carré et sans coche : la coche ronde est réservée à done. Une dépendance vers un batch est levée dès qu'il est submitted (version beta).
- Pas de cube pour une étape en cours, pas de cube à moitié rempli.
- Pas d'autre forme que le cube, la coche et le cercle barré.
