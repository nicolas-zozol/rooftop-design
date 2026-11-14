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
- RLOQ : puce pleine `rloq` / `rloq-texte`. Sur une tuile et au pied d'une epic repliée, la forme courte : disque blanc au point d'interrogation et le nombre (`title` « 1 RLOQ »), comme la puce de blocage. Dans une case du chantier, la forme longue « 1 RLOQ ». Dans une phrase, le disque `rloq` au point d'interrogation. Une RLOQ arrête le boss, une OQ non.
- Blocage : `blocage`, cercle barré `surface`, le nombre à côté.
- États : en cours `accent-pale` / `accent-fonce`, fait `fait-pale` / `fait-fonce`, tenu `etat-tenu` / `etat-tenu-texte`, hors batch `etat-hors-batch` / `etat-hors-batch-texte`. Chaque puce garde son icône.
- Lien OQ : « 3 OQ à répondre, dont 1 RLOQ » ou « OQ : aucune ouverte », flèche sortante.

## À éviter
- Pas de puce sans texte, pas de couleur d'état ailleurs que sur son état.
