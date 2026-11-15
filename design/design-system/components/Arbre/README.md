Vue Arbre : une ligne par epic sous le bandeau de son module, une colonne par soumission à venir, et le chemin vers un drapeau.

## L'appelant fournit
- pour chaque batch non fait : son état, ses stories, et ce qui doit être soumis avant qu'il puisse finir (le batch précédent de l'epic, les dépendances de ses stories, celle de son epic) ;
- le chemin à montrer : vers le drapeau d'un module, ou vers un batch cliqué.

## Quand l'utiliser
- Troisième onglet, après Questions et Blueprint. C'est la seule vue où l'on dessine des dépendances ; le plan n'en dessine pas.
- Colonnes « Fait », « Maintenant », « +1 », « +2 »… : un batch se place dans la première colonne où tout ce qui le relie à sa gauche est soumis. « Maintenant » est teintée (`.rt-colonne-maintenant`, `accent-pale`) : c'est là qu'on agit.
- Lignes dans l'ordre du plan, donc dans l'ordre de priorité.

## À faire
- Nœud `.rt-noeud` (116 × 58) : id court en mono, cubes des stories ou « tenu », compte, pastille d'état ; `.rt-noeud-fait` regroupe les batchs faits dans « Fait » (« B1–B3 ✓ »).
- Deux liens seulement : trait plein `cadre-arete` vers le batch suivant, tirets `blocage` coudés à angle droit avec flèche vers le batch qui attend. Un batch commencé qui ne peut finir que plus tard est relié à « Maintenant » par des pointillés `accent`.
- Le chemin passe en `selection` (trait 2,5 px, pastilles numérotées `.rt-etape-num`), le reste à 0,45, et le bandeau du bas l'écrit : « Chemin vers plugin-split, drapeau de RB Compound : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 → 3 B-PLUGIN-1 ».

## À éviter
- Pas de lien diagonal ni courbe, pas de lien pour « tenu par » en dehors de la ligne de l'epic.
- Le drapeau d'un autre module sur le chemin se signale, il ne se déplace pas.
