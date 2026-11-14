Rooftop montre l'avancement d'un projet comme une partie en cours : les modules et les epics sont posés sur un plan clair, chaque epic déroule son tapis de jeu, et chaque batch y est une tuile dans un cadre à emplacements. L'application est en lecture seule : elle montre, elle ne modifie rien. La seule chose que l'on copie, c'est la commande proposée dans le chantier.

## Voix et vocabulaire

- Écris en français, phrases courtes, au présent, sans point d'exclamation. Tutoie dans les consignes (« Modifie-la ici, puis copie. »).
- Garde le vocabulaire du projet tel quel : module, epic, batch, story, OQ, Hors batch. Ne traduis pas « story » ni « batch ».
- Nomme les états avec ces mots exacts : « en cours », « fait », « tenu par B1 », « bloquée », « hors batch », « terminée », « écartée ».
- Nomme l'étape d'une story avec ces libellés : « à écrire », « sans design », « design en brouillon », « design approuvé, à coder », « code fini, à valider puis soumettre », « soumise », « terminée ».
- Écris un blocage comme une cause et une suite : « attend B-TOOLMAN-2 », puis « Pour débloquer, soumettre dans l'ordre : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 ».
- Compte toujours : « 3 OQ à répondre », « OQ : aucune ouverte », « 4 stories », « 1 story ». Un nombre seul sur une puce garde son icône.
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
- Dessine une seule flèche sur le plan : « dépend de », en `fleche-dependance`, entre deux epics repliées. Les autres dépendances se lisent dans le chantier.
- Borde en tirets `tirets-bloquee` une epic bloquée et remplace sa pastille fuchsia par `accent-bloque`.

## Le plateau

- Peins le tapis en `tapis` avec un grain fin `tapis-fibre`, des ombres larges `tapis-ombre` et une couture en tirets `tapis-couture` à 6 px du bord. Le texte posé sur le tapis est en `tapis-texte`.
- Pose les batchs dans un `cadre` (rayon `radius-cadre`, marge `pad-cadre`, relief `ombre-cadre`), un emplacement de `taille-tuile` par batch, espacés de `gap-tuile`. Les emplacements vides sont des creux `emplacement` avec `ombre-emplacement`.
- Montre les seuls batchs de l'epic, dans l'ordre B1, B2… Un batch qui attend reste un emplacement vide gravé : son id, un cadenas, « tenu par B1 », son nombre de stories, en `emplacement-grave`.
- Une tuile en cours est en `tuile-en-cours`, soulevée de 2 px avec `ombre-tuile-en-cours`. Une tuile faite est en `tuile-faite`, à plat avec `ombre-tuile-posee`. La tuile sélectionnée monte de 5 px avec `ombre-tuile-selection` et un anneau `selection` décalé de 3 px.
- Sur une tuile, mets en haut à gauche l'id en mono (`id-tuile`), en haut à droite la pastille d'état, au milieu un cube par story, en bas la puce OQ, la puce de blocage s'il y en a, et le compte de stories en `tuile-muette`.
- Pose la pile **Hors batch** (`hors-batch`, liseré `hors-batch-lisere`, tranche `hors-batch-tranche`, texte `hors-batch-texte`) à `gap-hors-batch` du cadre, et seulement si elle contient des stories. Écris « Hors batch », jamais « pioche ».

## États et couleurs

| État | Pièce | Jetons |
|---|---|---|
| en cours | tuile soulevée, pastille fuchsia au marteau | `tuile-en-cours`, `accent` ; puce d'état `accent-pale` / `accent-fonce` |
| en cours, epic bloquée | même tuile, pastille grise | `accent-bloque` |
| fait | tuile à plat, pastille blanche au drapeau vert | `tuile-faite`, `fait`, `fait-fonce` ; puce `fait-pale` / `fait-fonce` |
| tenu | emplacement vide gravé, cadenas | `emplacement`, `emplacement-grave` ; puce `etat-tenu` / `etat-tenu-texte` |
| hors batch | pile bordeaux | `hors-batch` ; puce `etat-hors-batch` / `etat-hors-batch-texte` |

- Réserve `accent` (fuchsia) à « on agit ici » : la pastille d'une tuile en cours, le point d'une mini-tuile en cours, la puce « en cours ». Jamais en décor, jamais en fond large.
- Réserve `fait` (vert) à fait et terminée : drapeau, coche, cube terminé, puce « OQ ✓ ».
- Réserve `blocage` (rouge) aux blocages : puce « ⊖ n », cercle barré, texte `blocage-texte`, fond de ligne `blocage-fond`. Écris toujours le compte ou la cause à côté.
- Montre les OQ sur chaque tuile : `oq-ouverte` / `oq-ouverte-bord` / `oq-ouverte-texte` avec le nombre (« 3 OQ »), ou `oq-ok` / `oq-ok-bord` / `oq-ok-texte` (« OQ ✓ »). Dans le chantier, la puce devient un lien vers les OQ du batch.
- Dis l'avancement d'une epic par son anneau : `avancement-a-faire`, `avancement-en-cours`, `avancement-fini`, `avancement-fait`, sur piste `ligne-forte`. Le pourcentage est écrit à côté.
- `module-en-cours` (ambre) ne sert qu'au glyphe demi-plein du bandeau de module.

## Les étapes d'une story

- Représente chaque story par un cube de bois de `taille-cube` (`taille-cube-chantier` dans le chantier), rayon `radius-cube`, relief `ombre-cube`, espacés de `gap-cube`.
- Une couleur par étape atteinte : `etape-ecrite` (rose), `etape-design` (brun), `etape-code` (gris), `etape-soumise` (violet). Terminée devient une coche ronde `etape-terminee`.
- À écrire : cube vide en tirets `etape-a-ecrire`. Une étape à moitié faite (design en brouillon) : cube vide en tirets de la couleur de cette étape.
- Ne laisse jamais la couleur seule porter l'étape : le chantier écrit l'étape en toutes lettres dans la colonne « où elle en est ».

## Le chantier

- Ouvre le chantier sous le cadre, au clic sur une tuile ou sur la pile, à `marge-panneau` du cadre, avec une encoche pointée sur la pièce cliquée. Un clic sur la même pièce le referme.
- Peins-le en parchemin : fond `chantier`, bord `chantier-bord`, liseré `chantier-lisere` et filet `chantier-filet` (tous deux dans `ombre-panneau`), rayon `radius-panneau`.
- Titre : « Chantier B1 » en `chantier-titre` (style `chantier-titre`), puis la puce d'état, puis « B-PLUGIN-1 · 4 stories », puis le lien OQ à droite, puis la croix de fermeture. Pour la pile, le titre est « Hors batch ».
- Stories à gauche, en grille : slug en mono (`slug`), cinq cases `chantier-case` / `chantier-case-bord` pour écrite, design, code, soumise, terminée, puis « où elle en est ». Une story écartée est barrée, à `opacite-ecartee`.
- Dépendances et blocages à droite, colonne de `taille-dependances` séparée par un filet `chantier-filet` : verrous (cadenas), blocages (cercle barré rouge et chemin numéroté), « Aucun blocage venant d'une autre epic. » avec la coche verte.
- Action en bas, sous un tiret `chantier-separateur` : titre « Action » (style `action-titre`), la phrase « Proposition complète pour avancer B-PLUGIN-1. Modifie-la ici, puis copie. », les boutons « Rétablir » (contour `chantier-bord`) et « Copier » (plein `chantier-bouton`, texte `chantier-lisere`), et l'éditeur `chantier-editeur` en mono (`editeur`). La première ligne de l'Action est toujours la commande à lancer.
- Un batch tenu n'a pas d'Action : écris « Aucune action tant que B1 n'est pas soumis. »

## Le chemin de déblocage

- Quand on demande comment débloquer une story, affiche en haut du plan le bandeau `inverse` : « Pour débloquer plugin-split, soumettre : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 », numéros en pastilles `texte-inverse`, bouton « Quitter le chemin » en `inverse-2`.
- Numérote les étapes sur les tuiles mêmes, pastille `selection` bordée de `surface`, en haut à gauche.
- Estompe tout ce qui n'est pas sur le chemin : epics, tuiles et pile à `opacite-estompe`, mini-tuiles à `opacite-mini-estompe`. Ne masque rien.

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

- Dessine les icônes en SVG inline, trait `currentColor` de 1,3 à 1,6, extrémités arrondies, sur une grille de 12 ou 14 : cadenas (tenu), marteau (en cours), drapeau (fait), coche dans un cercle (terminée, OQ ✓), cercle barré (blocage), cible (« Voir sur le plan »), copie, croix, flèche sortante (lien OQ), chevron (replier), grille (module).
- Marque `aria-hidden="true"` toute icône doublée d'un texte, et donne un `aria-label` à celle qui est seule (la pastille d'une tuile : « en cours », « fait »).
- Rooftop n'a pas de logo dans ses sources : écris le nom en texte.

## Contraste et accessibilité

- Les paires de texte du système passent 4,5:1, sauf trois, gardées telles quelles des maquettes validées : le texte gravé `emplacement-grave` sur `emplacement` (2:1, voulu pour l'effet gravé), « terminée » en `fait-bord` sur `chantier` (3,8:1, doublé par la coche) et les traits `tirets-bloquee` et `fleche-dependance` (2,5:1, doublés par le texte « dépend de »). Fonce-les si la lisibilité gêne.
- L'anneau de focus est `selection`, plein, 2,5 px, décalé de 3 px : 11:1 sur `cadre`, 9:1 sur `emplacement`.
- Les tuiles et la pile sont des boutons : `aria-pressed` quand le chantier est ouvert sur elles, `title` avec l'état complet (« B1 · en cours · 4 stories · 3 OQ à répondre »).
- Un état ne repose jamais sur la seule couleur : chaque pastille a son icône, chaque puce son nombre, chaque étape son libellé dans le chantier.

## Utiliser le système

- Dans le canevas : copie `tokens.css` (et `components/bundle.css` si tu veux les classes `rt-`) dans `ds/rooftop/`, lie `ds/rooftop/tokens.css` dans chaque planche et écris `var(--nom)` partout où une maquette mettait un hexadécimal. Changer une couleur dans ce système puis recopier les fichiers met toutes les planches à jour.
- Dans un SVG, passe la couleur par `style="fill:var(--tapis)"` ou `style="stroke:var(--fait)"` : un attribut `fill="var(--…)"` n'est pas lu.
- Dans l'application (React) : importe `tokens.css` une fois à la racine, utilise les classes de style typographique (`.slug`, `.chantier-titre`…) ou les variables `--font-…`, et compose les pièces avec les classes `rt-` de `components/bundle.css`.
