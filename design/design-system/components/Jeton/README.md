Puces, pastilles et liens d'état : OQ, blocage, état d'un batch, lien vers les OQ.

## L'appelant fournit
- un nombre (OQ ouvertes, RLOQ, stories bloquées) ou un état (en cours, fait, tenu par B1, hors batch) ;
- pour le lien OQ, l'ancre des questions du batch (`#oq-b-plugin-1`).

## Quand l'utiliser
- Puce OQ et puce de blocage : en bas d'une tuile et dans le pied d'une epic repliée.
- Pastille : en haut à droite d'une tuile (`taille-pastille`).
- Puce d'état et lien OQ : dans l'en-tête du chantier.

## À faire
- OQ ouvertes : `oq-ouverte` / `oq-ouverte-bord` / `oq-ouverte-texte`, « 3 OQ ». Aucune : `oq-ok` / `oq-ok-bord` / `oq-ok-texte`, « OQ ✓ ».
- Toutes les puces se lisent pareil : le nombre d'abord, puis ce qu'il compte. « 3 OQ », « 1 RLOQ » (puce pleine `rloq` / `rloq-texte`, le mot écrit, jamais un symbole), « 1 ⊖ » (le cercle barré après le nombre). Dans une liste, la petite puce « RLOQ » sert d'icône. Une RLOQ arrête le boss, une OQ non.
- Blocage : `blocage`, le nombre puis le cercle barré `surface`.
- États : en cours `accent-pale` / `accent-fonce`, fait `fait-pale` / `fait-fonce`, tenu `etat-tenu` / `etat-tenu-texte`, hors batch `etat-hors-batch` / `etat-hors-batch-texte`. Chaque puce garde son icône.
- Lien OQ : « 3 OQ à répondre, dont 1 RLOQ » ou « OQ : aucune ouverte », flèche sortante.

## À éviter
- Pas de puce sans texte, pas de couleur d'état ailleurs que sur son état.
