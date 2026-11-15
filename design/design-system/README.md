Rooftop montre l'avancement d'un projet comme une partie en cours : les modules et les epics sont posés sur un plan clair, chaque epic déroule son tapis de jeu, et chaque batch y est une tuile dans un cadre à emplacements. L'application est en lecture seule : elle montre, elle ne modifie rien. La seule chose que l'on copie, c'est la commande proposée dans le chantier.

## Voix et vocabulaire

- Écris en français, phrases courtes, au présent, sans point d'exclamation. Tutoie dans les consignes (« Modifie-la ici, puis copie. »).
- Garde le vocabulaire du projet tel quel : module, epic, batch, story, OQ, RLOQ, Hors batch, boss. Ne traduis pas « story » ni « batch ». Une OQ est une question ouverte à laquelle le boss peut répondre lui-même ; une RLOQ (ReLevant Open Question) ne se tranche que par un humain et arrête le boss.
- Nomme les états d'un batch avec ces mots exacts : « en cours », « fait », « tenu par B1 », « bloquée », « hors batch ». Une story retirée est « écartée ».
- Nomme les étapes d'une story en anglais, avec ces mots exacts et dans cet ordre : ready, brainstorm, design file, code, submitted, done. N'écris jamais « design » seul pour « design file », ni « terminée » ou « soumise ». Une story qui n'a pas encore atteint ready est « pas ready ».
- Écris un blocage comme une cause et une suite : « attend B-TOOLMAN-2 », puis « Pour débloquer, soumettre dans l'ordre : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 ».
- Compte toujours : « 3 OQ à répondre, dont 1 RLOQ », « OQ : aucune ouverte », « 4 stories », « 1 story ». Un nombre seul sur une puce garde son icône.
- Mets en capitales les seules étiquettes INFIX (PLUGIN, TOOLMAN) et le surtitre MODULE.
- N'utilise ni emoji ni pictogramme décoratif. Les seuls glyphes de texte admis sont « → », « · », « ✓ » et « ⊖ ».

## Deux surfaces, deux registres

- **Le plan** est l'interface : fond `surface-plan` semé de points `surface-plan-point` tous les 18 px, cartes en `surface`, cadres de module en `surface-module`, texte en `encre`, `encre-2` et `encre-muette`, filets en `ligne`. Inter, sobre, aucun relief au-delà de `ombre-carte`.
- **Le plateau** est le jeu : `tapis` sous les pièces, `cadre` et `emplacement` en gris, tuiles en carton `tuile-en-cours` ou `tuile-faite`, parchemin `chantier` pour le détail. Le relief y porte le sens.
- Ne mélange pas les deux : pas de carton ni d'Alegreya sur le plan, pas de carte blanche plate sur le tapis.

## Le plan

- Pose chaque module dans un cadre `surface-module`, bord `ligne`, rayon `radius-module`, coiffé d'un bandeau `bandeau` de `taille-en-tete` (40 px) et d'une barre d'avancement de `taille-barre-module` (piste `bandeau-piste`, plein `bandeau-texte`).
- Une epic seule dans son module fusionne le bandeau en haut de sa carte : ne dessine pas deux en-têtes empilés.
- Espace les modules de `gap-module`, les epics d'un même module de `pad-tapis`, l'intérieur d'un module de `pad-module`.
- Une epic repliée fait `taille-epic-repliee` (240 px) de large. Une epic déroulée prend la largeur de son cadre, et au moins `taille-chantier` quand le chantier est ouvert.
- Ne dessine aucune flèche de dépendance sur le plan : il y en aurait trop. Montre seulement la dépendance qui bloque l'item en cours, sur la carte de l'epic qu'elle bloque : « ⊖ attend BATCHES » en `blocage-texte`. Les liens se dessinent dans l'Arbre, et seulement là.
- Borde en tirets `tirets-bloquee` une epic bloquée et remplace sa pastille sarcelle par `accent-bloque`.

## Le plateau

- Peins le tapis en `tapis` avec un grain fin `tapis-fibre`, des ombres larges `tapis-ombre` et une couture en tirets `tapis-couture` à 6 px du bord. Le texte posé sur le tapis est en `tapis-texte`.
- Pose les batchs dans un `cadre` (rayon `radius-cadre`, marge `pad-cadre`, relief `ombre-cadre`), un emplacement de `taille-tuile` par batch, espacés de `gap-tuile`. Les emplacements vides sont des creux `emplacement` avec `ombre-emplacement`.
- Montre les seuls batchs de l'epic, dans l'ordre B1, B2… Un batch qui attend reste un emplacement vide gravé : son id, un cadenas, « tenu par B1 », son nombre de stories, en `emplacement-grave`.
- Une tuile en cours est en `tuile-en-cours`, soulevée de 2 px avec `ombre-tuile-en-cours`. Une tuile faite est en `tuile-faite`, à plat avec `ombre-tuile-posee`. La tuile sélectionnée monte de 5 px avec `ombre-tuile-selection` et un anneau `selection` décalé de 3 px.
- Sur une tuile, mets en haut à gauche l'id en mono (`id-tuile`), en haut à droite la pastille d'état, au milieu un cube par story, en bas la puce OQ, la puce RLOQ et la puce de blocage s'il y en a, et le compte de stories en `tuile-muette`.
- Pose la pile **Hors batch** (`hors-batch`, liseré `hors-batch-lisere`, tranche `hors-batch-tranche`, texte `hors-batch-texte`) à `gap-hors-batch` du cadre, et seulement si elle contient des stories. Écris « Hors batch », jamais « pioche ».

## États et couleurs

| État | Pièce | Jetons |
|---|---|---|
| en cours | tuile soulevée, pastille sarcelle à l'engrenage | `tuile-en-cours`, `accent` ; puce d'état `accent-pale` / `accent-fonce` |
| en cours, epic bloquée | même tuile, pastille grise | `accent-bloque` |
| fait | tuile à plat, pastille blanche au drapeau vert | `tuile-faite`, `fait`, `fait-fonce` ; puce `fait-pale` / `fait-fonce` |
| tenu | emplacement vide gravé, cadenas | `emplacement`, `emplacement-grave` ; puce `etat-tenu` / `etat-tenu-texte` |
| hors batch | pile bordeaux | `hors-batch` ; puce `etat-hors-batch` / `etat-hors-batch-texte` |

- Réserve `accent` (sarcelle) à « on agit ici » : la pastille d'une tuile en cours, le point d'une mini-tuile en cours, la puce « en cours ». Jamais en décor, jamais en fond large.
- Réserve `fait` (vert) à fait et done : drapeau, coche done, puce « OQ ✓ ».
- Réserve l'orange (`oq-ouverte-*`, `rloq`, `etape-pas-ready`) aux questions et le rouge (`blocage-*`) aux dépendances bloquantes. Une étape qui avance ne prend jamais ces couleurs.
- Réserve `blocage` (rouge) aux blocages : puce « ⊖ n », cercle barré, texte `blocage-texte`, fond de ligne `blocage-fond`. Écris toujours le compte ou la cause à côté.
- Écris toutes les puces dans le même ordre : le nombre, puis ce qu'il compte. « 3 OQ », « 1 RLOQ » (le mot, jamais un symbole), « 1 ⊖ ».
- Montre les OQ sur chaque tuile : `oq-ouverte` / `oq-ouverte-bord` / `oq-ouverte-texte` avec le nombre (« 3 OQ »), ou `oq-ok` / `oq-ok-bord` / `oq-ok-texte` (« OQ ✓ »). S'il y a des RLOQ, ajoute la puce pleine `rloq` / `rloq-texte` (« 1 RLOQ »). Dans le chantier, la puce devient un lien vers les OQ du batch.
- Calcule l'avancement, ne le saisis jamais : une story vaut ses étapes franchies sur six (design file = 3/6 = 50 %, done = 100 %) ; un batch, la moyenne de ses stories (tenu 0 %, fait 100 %, stories écartées ignorées) ; une epic, la moyenne de ses batchs ; un module, la moyenne de ses epics. Une epic sans batch est à 0 %. Exemple : quatre batchs dont un seul a toutes ses stories au design file : 50 % / 4 = 12,5 %.
- Dis l'avancement d'une epic par son anneau : `avancement-a-faire` (0 %), `avancement-en-cours`, `avancement-fini` (100 %, coche dans l'anneau), `avancement-fait` (mergée), sur piste `ligne-forte`. Le pourcentage est écrit à côté.
- Garde les mêmes faits partout : la tuile, sa mini-tuile, le pied de l'epic repliée et le chantier lisent les mêmes stories. Un batch est fait seulement quand toutes ses stories sont done ; une epic bloquée a la pastille `accent-bloque` sur sa tuile et la puce « attend BATCHES » dans son chantier.
- `module-en-cours` (ambre) ne sert qu'au glyphe demi-plein du bandeau de module.
- Réserve `priorite` (doré) et `priorite-bord` au fanion du drapeau de priorité, et `priorite-pale` au survol des cibles en mode Drapeau. Le doré ne dit jamais un état.

## Les étapes d'une story

- Une story existe dès qu'elle est écrite. Elle est **ready** quand toutes ses OQ ont une réponse (definition of ready). Viennent ensuite brainstorm (qui fait souvent naître des questions), design file, code, submitted, done.
- Représente chaque story par un cube de bois de `taille-cube` (`taille-cube-chantier` dans le chantier), rayon `radius-cube`, relief `ombre-cube`, espacés de `gap-cube`.
- Colore le cube de la dernière étape franchie, du plus terne au plus flashy : `etape-ready` (gris clair), `etape-brainstorm` (bleu clair), `etape-design-file` (bleu vif), `etape-code` (mauve), `etape-submitted` (le vert de done, en cube carré sans coche). Done finit la progression : coche ronde verte `etape-done`.
- Chaque étape change de teinte, pas seulement de valeur : deux étapes voisines doivent se distinguer sur une tuile isolée. N'ajoute pas d'étape dans une teinte déjà prise.
- Pas ready : cube vide en tirets orange `etape-pas-ready`. C'est le seul cube vide ; une étape en cours ne se dessine pas, seule compte la dernière franchie.
- Sur une tuile, un cube par story, à la couleur de sa dernière étape franchie. La teinte dit l'étape ; le chantier la confirme colonne par colonne.
- Une case peut être bloquée de trois façons, chacune posée dans la case même : des OQ (puce `oq-ouverte`, « 2 OQ » : le boss y répond et continue), des RLOQ (puce pleine `rloq`, « 1 RLOQ » : le boss s'arrête), une dépendance (cercle barré `blocage`, `title` « design file bloqué : attend B-TOOLMAN-2 » : le boss s'arrête). Pas ready = la case ready porte des OQ ou des RLOQ.
- Une dépendance vers un batch est levée dès que ce batch est submitted (il sert de version beta) : n'attends pas done.
- Une dépendance ne compte que si elle bloque une case que le boss va atteindre. Un blocage plus loin que le premier arrêt n'est pas bloquant aujourd'hui : ne le montre pas comme tel.
- Après le premier arrêt d'une story, passe ses cases à `opacite-hors-course` : un « boss atw » ne les atteindra pas.

## Le chantier

- Ouvre le chantier sous le cadre, au clic sur une tuile ou sur la pile, à `marge-panneau` du cadre, avec une encoche pointée sur la pièce cliquée. Un clic sur la même pièce le referme.
- Peins-le en parchemin : fond `chantier`, bord `chantier-bord`, liseré `chantier-lisere` et filet `chantier-filet` (tous deux dans `ombre-panneau`), rayon `radius-panneau`.
- Titre : « Chantier B1 » en `chantier-titre` (style `chantier-titre`), puis la puce d'état, puis « B-PLUGIN-1 · 4 stories », puis le lien OQ à droite, puis la croix de fermeture. Pour la pile, le titre est « Hors batch ».
- Stories à gauche, en grille : slug en mono (`slug`), puis six cases `chantier-case` / `chantier-case-bord` titrées ready, brainstorm, design file, code, submitted, done. Chaque étape franchie a son cube ; pas ready, la case ready porte le cube en tirets orange ; une étape bloquée porte le cercle barré. Pas d'autre colonne. Une story écartée est barrée, à `opacite-ecartee`.
- À droite, colonne « Où le boss s'arrête », de `taille-dependances`, séparée par un filet `chantier-filet` : une ligne par story arrêtée (icône RLOQ ou cercle barré, la case, la raison), puis une ligne cochée pour les stories qui vont jusqu'au bout, puis le chemin numéroté d'une dépendance et « Voir sur le plan ». Pas de verrou sur les batchs suivants : les emplacements tenus le disent déjà.
- Action en bas, sous un tiret `chantier-separateur` : titre « Action » (style `action-titre`), la phrase « Proposition complète pour avancer B-PLUGIN-1. Modifie-la ici, puis copie. », les boutons « Rétablir » (contour `chantier-bord`) et « Copier » (plein `chantier-bouton`, texte `chantier-lisere`), et l'éditeur `chantier-editeur` en mono (`editeur`). La première ligne de l'Action est toujours la commande à lancer, en général « boss atw B-PLUGIN-1 » ; la suite dit où le boss va s'arrêter, story par story.
- Un batch tenu n'a pas d'Action : écris « Aucune action tant que B1 n'est pas soumis. »

## Le chemin de déblocage

- Quand on demande comment débloquer une story, affiche en bas du plan le bandeau `inverse` : « Pour débloquer le design file de plugin-split, soumettre : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 », numéros en pastilles `texte-inverse`, bouton « Quitter le chemin » en `inverse-2`.
- Numérote les étapes sur les tuiles mêmes, pastille `selection` bordée de `surface`, en haut à gauche.
- Estompe tout ce qui n'est pas sur le chemin : epics, tuiles et pile à `opacite-estompe`, mini-tuiles à `opacite-mini-estompe`. Ne masque rien.

## Priorités : le drapeau, l'ordre, Ranger

- Lis le plan comme une page : en haut à gauche, le plus prioritaire. Les modules se suivent dans cet ordre, puis les epics dans leur module. On règle l'ordre à la main, en mode Déplacer.
- Chaque module porte au plus un drapeau de priorité, le fanion doré, posé sur une de ses epics, un de ses batchs ou une de ses stories. Le reposer ailleurs dans le module le déplace ; recliquer sur sa cible le retire.
- Montre le drapeau partout où sa cible se voit : en-tête d'epic, tuile et mini-tuile (aussi quand il est sur une de leurs stories), ligne de story du chantier, et la puce du bandeau de module, qui nomme la cible en mono (« plugin-split », « B-TOOLMAN-1 »).
- Trois modes dans la barre du plan, chacun avec sa touche : **Ouvrir** (V, par défaut : un clic sur le bandeau d'une epic l'ouvre ou la ferme), **Déplacer** (M : on glisse les epics et les modules, ou on les sélectionne puis ← →), **Drapeau** (F : outil ponctuel, on clique la cible, puis on revient au mode d'avant). Échap annule.
- **Ranger par priorité** (R pour tout le plan, « Ranger » dans le bandeau pour un module) remet les epics du module dans cet ordre : les prérequis de l'epic au drapeau qui sont dans le module, l'epic au drapeau, les epics en cours de la plus avancée à la moins avancée (finir avant de commencer), les pas commencées, les bloquées, les finies. Sans drapeau, on commence aux epics en cours.
- Après une pose ou un rangement, écris le bilan en bas du plan, avec « Annuler » : le nouvel ordre numéroté et la raison de chaque place (« prérequis de RTWORKTREES », « en cours · 92 % », « pas commencée », « attend BATCHES »). Si l'epic au drapeau attend un autre module, dis-le : « PLUGIN attend aussi B-TOOLMAN-2, dans le module Toolman, qui garde sa propre priorité », et renvoie à l'Arbre.
- La priorité reste par module, même quand l'arbre traverse les modules : Rooftop montre le lien, il ne touche jamais au drapeau d'un autre module.

## L'Arbre

- Troisième vue, après Questions et Blueprint, et la seule où l'on dessine des dépendances.
- Une ligne par epic, sous le bandeau de son module, dans l'ordre du plan. Une colonne par soumission à venir : « Fait », « Maintenant », « +1 », « +2 »… Un batch se place dans la première colonne où tout ce qui le relie à sa gauche est soumis ; un batch commencé qui ne peut finir que plus tard est relié à « Maintenant » par des pointillés `accent`.
- Nœuds en petit carton (116 × 58) : id court en mono, cubes des stories ou « tenu », compte, pastille d'état. Les batchs faits se regroupent dans « Fait » (« B1–B3 ✓ »). Une epic sans batch montre sa commande dans un cadre en tirets.
- Deux liens seulement : trait plein `cadre-arete` vers le batch suivant de l'epic ; tirets `blocage` coudés à angle droit, flèche vers le batch qui attend, pour une dépendance bloquante.
- En haut, choisis le chemin : vers le drapeau d'un module, ou vers un batch cliqué. Le chemin passe en `selection` (trait 2,5 px, pastilles numérotées), le reste à 0,45 ; le bandeau du bas l'écrit (« Chemin vers plugin-split, drapeau de RB Compound : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 → 3 B-PLUGIN-1 ») et signale un drapeau d'un autre module sur le chemin.
- Le mode Drapeau (F) y marche aussi : clic sur un batch, ou sur le nom d'une epic.

## Typographie

- Inter (`font-sans`) pour toute l'interface. Ne dépasse jamais 15 px dans l'application : `module` pour le nom du module, `titre-epic` pour l'epic, `texte` pour les phrases, `texte-petit` pour les barres, boutons et puces d'état, `legende` pour les colonnes et le texte gravé, `puce` pour les puces OQ et blocage, `infix` et `surtitre` en capitales espacées.
- JetBrains Mono (`font-mono`) pour les ids, slugs, commandes et branches, et seulement pour eux : `id-emplacement`, `id-tuile`, `slug`, `commande`, `editeur`.
- Alegreya (`font-jeu`) seulement sur le parchemin du chantier et la pile Hors batch : `chantier-titre`, `action-titre`, `chantier-section`, `hors-batch-titre`.
- Coupe un slug trop long par une ellipse, jamais par un retour à la ligne. Le slug entier reste dans le `title`.

## Espacements, rayons, relief

- Prends les espacements tels quels, sans grille imposée : `gap-cube` 4, `gap-tuile` 8, `pad-cadre` 10, `pad-en-tete` 12, `pad-module` 16, `marge-panneau` 18, `pad-tapis` 20, `gap-hors-batch` 24, `gap-module` 28.
- Garde le carton presque carré (`radius-cube`, `radius-tuile`) et arrondis les contenants (`radius-cadre`, `radius-epic`, `radius-module`). Pastilles et coches en `radius-rond`, puces en `radius-puce`.
- Le relief dit l'état : soulevée = en jeu, à plat = faite, en creux = en attente. N'ajoute pas d'ombre décorative ailleurs.
- Les dimensions des pièces sont fixes : `taille-tuile` 104, `taille-mini-tuile` 24, `taille-pastille` 24, `taille-en-tete` 40. Une tuile ne s'étire pas.

## Iconographie

- Dessine les icônes en SVG inline, trait `currentColor` de 1,3 à 1,6, extrémités arrondies, sur une grille de 12 ou 14 : cadenas (tenu), engrenage plein (en cours), drapeau (fait), coche dans un cercle (done, OQ ✓), cercle barré (dépendance bloquante), pour une RLOQ, la puce « RLOQ » écrite, jamais un point d'interrogation, cible (« Voir sur le plan »), copie, croix, flèche sortante (lien OQ), chevron (replier), grille (module), fanion doré (drapeau de priorité, plein, jamais au trait), flèche de pointeur (Ouvrir), main (Déplacer), poignée à six points (pièce qu'on déplace), lignes et flèche vers le bas (Ranger), chevrons ‹ › (rang).
- Marque `aria-hidden="true"` toute icône doublée d'un texte, et donne un `aria-label` à celle qui est seule (la pastille d'une tuile : « en cours », « fait »).
- Rooftop n'a pas de logo dans ses sources : écris le nom en texte.

## Contraste et accessibilité

- Les paires de texte du système passent 4,5:1, sauf deux, gardées telles quelles des maquettes validées : le texte gravé `emplacement-grave` sur `emplacement` (2:1, voulu pour l'effet gravé) et les tirets `tirets-bloquee` d'une epic bloquée (2,5:1, doublés par « attend BATCHES » en rouge). Fonce-les si la lisibilité gêne.
- Le cube `etape-ready` est clair sur le carton (1,4:1) : son bord et son relief le détachent. `etape-design-file` et `etape-code` ont presque la même clarté : pour un daltonien, le bleu et le mauve peuvent se confondre ; la colonne du chantier tranche.
- Submitted et done partagent le vert `fait` : submitted est un cube carré sans coche, done une coche ronde. La forme les sépare, garde-la ; n'ajoute pas un second vert.
- L'anneau de focus est `selection`, plein, 2,5 px, décalé de 3 px : 11:1 sur `cadre`, 9:1 sur `emplacement`.
- Les tuiles et la pile sont des boutons : `aria-pressed` quand le chantier est ouvert sur elles, `title` avec l'état complet (« B1 · en cours · 4 stories · 3 OQ à répondre »).
- Un état ne repose jamais sur la seule couleur : chaque pastille a son icône, chaque puce son nombre, chaque étape sa colonne nommée dans le chantier et un `title` sur sa case.
- Le plan prend le focus pour ses raccourcis (V, M, F, R, Échap, ← → en Déplacer) ; chaque bouton de mode porte `aria-pressed` et montre sa touche. Glisser a toujours son équivalent : les flèches de la pastille de rang et le clavier. Les cibles du mode Drapeau sont des boutons, soulignées aussi au focus.

## Utiliser le système

- Dans le canevas : copie `tokens.css` (et `components/bundle.css` si tu veux les classes `rt-`) dans `ds/rooftop/`, lie `ds/rooftop/tokens.css` dans chaque planche et écris `var(--nom)` partout où une maquette mettait un hexadécimal. Changer une couleur dans ce système puis recopier les fichiers met toutes les planches à jour.
- Dans un SVG, passe la couleur par `style="fill:var(--tapis)"` ou `style="stroke:var(--fait)"` : un attribut `fill="var(--…)"` n'est pas lu.
- Dans l'application (React) : importe `tokens.css` une fois à la racine, utilise les classes de style typographique (`.slug`, `.chantier-titre`…) ou les variables `--font-…`, et compose les pièces avec les classes `rt-` de `components/bundle.css`.
