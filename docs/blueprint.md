# Blueprint — spécification de conception

> **Direction validée : tuiles sobres sur tapis de jeu** (section 9). Les sections 1 à 7 restent valables pour la hiérarchie, les états, le chemin de déblocage et le contrat backend ; leur traitement graphique est remplacé par la section 9.

État au commit qui porte ce fichier. Les maquettes de référence sont dans le canevas (voir le README) ; ce document en garde les décisions par écrit.

Rooftop est une app de bureau Tauri, **en lecture seule**. Le Blueprint montre un graphe React Flow : modules → epics → batchs → stories. Libellés en français ; identifiants, slugs et commandes en **mono** (JetBrains Mono).

---

## 1. Quatre niveaux, quatre signatures

Chaque niveau se reconnaît par quatre indices à la fois : icône, étiquette, surface, forme de l'avancement.

| Niveau | Icône | Étiquette | Surface | Avancement |
|---|---|---|---|---|
| **Module** | grille 2×2 | `MODULE` (9,5 px, 700, espacée) | bandeau gris foncé 40 px + barre blanche 4 px | barre de 4 px sous le bandeau, glyphe d'état clair |
| **Epic** | drapeau | **INFIX** en pastille grise (`PLUGIN`, `QUALITYGATES`, `TRWG`) | carte blanche, 240 px | anneau 16 px, trait 2,2, couleur de l'état |
| **Batch** | trois lignes | puce blanche cerclée `B1` | surface gris clair dans la carte | mini-barre : un segment 10 × 4 par story |
| **Story** | aucune | aucune | aucune — texte mono nu, 12/400 | cinq pips (7 px, écart 3) |

- L'INFIX **remplace** l'identifiant de l'epic : c'est lui qu'on lit.
- **Module à une seule epic** : bandeau fusionné — le bandeau du module devient le haut de la carte d'epic (`CarteEpic` avec `module`, `modetat`, `modpct`). Exemple : `trwg`.
- La story est le seul élément en mono dans le corps de la carte : c'est ce qui la distingue, sans cadre ni fond.

## 2. États et couleurs

Une seule couleur d'accent ; chaque couleur a un sens unique.

| Signe | Sens |
|---|---|
| **Fuchsia** (`#d946ef`, texte `#a21caf`) | « on agit ici ». Descend au niveau le plus fin visible : anneau sur la carte repliée ; epic déroulée → anneau sur le batch courant, cadre de l'epic neutre. Une epic bloquée n'a pas de fuchsia. |
| **Rouge + ⊖** (`#dc2626`, texte `#b91c1c`) | blocage, toujours avec son compte (« 1 blocage »). |
| **Cadenas gris** | verrou : le batch attend que le précédent soit **soumis**. Pas un blocage visuel. |
| **Pointillé** | attend ce qui est à sa gauche (batch tenu, epic bloquée, « Sans module »). |
| **Contour noir 2 px, décalé de 3** | sélection, distincte de l'anneau fuchsia. |
| Ambre / vert / bleu | en cours / tout juste fini / fait (valeurs Tailwind supposées, à caler sur le code). |

États de batch (`CarteBatch.etat`) : `courant`, `fait`, `tenu` (par le batch précédent), `hors` (hors batch).
Avancement d'une story (pips, ancienne direction) : 0 à écrire · 1 sans design · 1,5 design en brouillon · 2 design approuvé, à coder · 3 code fini, à valider puis soumettre · 4 soumise · 5 terminée · −1 écartée. Remplacé par les six étapes de la section 11.

## 3. Dérouler une epic

- Le coin haut gauche reste fixe ; les voisines glissent en 200 ms ; le zoom ne change pas.
- Le plan ne défile que pour montrer le batch courant en entier.
- Batch courant ouvert (320 px), les suivants en pastilles 104 × 52, reliés par verrou (56), trait plein (24) ou pointillé (14).
- Batch courant à 8 stories : toutes les lignes restent visibles, la carte s'allonge.
- Rail de l'epic repliée à 5 batchs et plus : segments égaux qui se resserrent, repère `B4/9` à droite.

## 4. Dépendances entre epics et modules

**Pas de flèches permanentes.** Un batch lointain (`B-PLUGIN-5`) est bloqué par beaucoup de choses ; ce qui est utile, c'est de savoir quoi débloquer pour un batch proche du but.

- Le **backend** calcule le graphe et le chemin ; Rooftop ne fait qu'afficher.
- Sur le plan : une story qui dépend d'une autre epic montre « **⊖ n à débloquer** » si n ≤ 3 ; au-delà, un ⊖ seul.
- Au clic : les batchs à soumettre reçoivent des **étapes numérotées** 1, 2…, le reste du plan est estompé (opacité 0,35). L'étape 1 porte le fuchsia et sa commande dans l'inspecteur.
- Les étapes hors de la vue sont signalées au bord du plan (pastilles de bord).
- **Lever** une dépendance = **soumettre** le batch, comme pour le verrou.
- **Branches parallèles** : le chiffre donne le rang, la lettre la branche (`1a`, `1b`, puis `2`). Toutes les étapes lançables d'un même rang portent le fuchsia.

Exemple : `plugin-split` (B-PLUGIN-1) dépend de `B-TOOLMAN-2`, lui-même tenu par `B-TOOLMAN-1`.
Inspecteur : « Pour débloquer · 2 batchs à soumettre » → 1. B-TOOLMAN-1 (commande, « Voir sur le plan ») → 2. B-TOOLMAN-2 (tenu par B-TOOLMAN-1) → « plugin-split débloquée ».

### Contrat backend proposé

```json
{
  "story": "plugin-split",
  "aDebloquer": [
    { "rang": 1, "branche": "a", "batch": "B-TOOLMAN-1", "module": "Toolman", "etat": "courant",
      "action": "soumettre", "stories": ["…"], "commande": "storyman …" },
    { "rang": 2, "branche": null, "batch": "B-TOOLMAN-2", "module": "Toolman", "etat": "tenu",
      "action": "soumettre", "stories": ["…"], "commande": null }
  ]
}
```

- Liste ordonnée ; `commande` est `null` quand l'étape n'est pas encore lançable.
- `branche` n'est renseignée que lorsque plusieurs étapes partagent un rang.
- Le compte affiché sur le plan est `aDebloquer.length`.

Les cinq blocages de l'inspecteur sont sur la planche « Inspecteur, blocages, tiroir, légende » (le cinquième : « dépend de B-TOOLMAN-2 »).

## 5. Mobile (à construire plus tard)

Rien à construire aujourd'hui ; la planche 05 dit ce que le desktop ne doit pas empêcher.

1. **Le plan devient une liste.** Même hiérarchie : bandeau de module, carte d'epic pleine largeur, batch courant déplié dessous, les autres batchs dans le rail. Pas de graphe : « dépend de INFIX » suffit entre epics.
2. **Le chemin est déjà une liste.** Le composant de l'inspecteur passe tel quel dans une feuille du bas — c'est pour cela que le chemin n'est pas dessiné en flèches.
3. **Rien n'existe qu'au survol.** Chaque info du survol a sa place au toucher, dans l'inspecteur.
4. **Cibles tactiles.** Ligne de story 44 px, bande de commande 48 px, bouton « Copier la commande » pleine largeur dans la feuille.
5. **Composants à largeur fluide.** Les tailles de la planche sont des minimums, pas des valeurs figées.
6. **Le calcul reste au backend.** Le téléphone reçoit le chemin déjà ordonné.

## 6. Tailles

| Élément | Taille |
|---|---|
| Carte d'epic repliée | 240 × 116 |
| En-tête d'epic déroulée | 40, corps marge 14 |
| Batch ouvert | 320 de large |
| Pastille de batch | 104 × 52 (courant : 150) |
| Ligne de story | 26 de haut |
| Pip | 7, écart 3 (inspecteur : 10) |
| Segment de rail | 6 de haut, écart 3 |
| Bande de commande | 32, bouton 26 |
| Cadre de module | bandeau 40 + barre 4 · marge 16 |
| Inspecteur | 400 de large |
| Anneau d'epic | 16, trait 2,2 |
| Mini-barre de batch | 10 × 4 par story, écart 2 |
| Barre du haut, barre du bas | 48 · 36 |

## 7. Jetons (clair / sombre)

| Jeton | Clair | Sombre |
|---|---|---|
| plan | `#f3f3f3` | `#0f0f0f` |
| cadre de module | `#fafafa` | `#141414` |
| carte | `#ffffff` | `#1c1c1c` |
| bandeau de module | `#262626` | `#333333` |
| étiquette INFIX | `#ededed` | `#2f2f2f` |
| surface de batch | `#f4f4f4` | `#262626` |
| bande de commande | `#ececec` | `#2f2f2f` |
| pointillé | `#a3a3a3` | `#5f5f5f` |
| accent | `#d946ef` | `#e879f9` |
| blocage | `#dc2626` | `#ef4444` |

Les jetons complets sont dans la méthode `tok(dark)` de chaque composant.

## 8. Ouvert : trois pistes graphiques pour les batchs

Le traitement des batchs (panneau gris à cadre rose) et le mode sombre sont jugés trop génériques. Trois pistes, page *Pistes batchs* du canevas, chacune en clair et en sombre. L'existant n'a pas bougé. Constantes : bandeau de module noir, en-tête d'epic, Inter / mono, fuchsia = on agit ici, rouge ⊖ = blocage, mêmes données d'exemple.

| | Tenu (pas commencé) | Courant (en cours) | Fait | Sombre |
|---|---|---|---|---|
| **A · Planche à dessin** | croquis au crayon, traits qui débordent aux angles | mise à l'encre : le trait d'encre fait le tour du cadre au prorata de l'avancement des stories, une plume marque où il s'arrête ; numéro entouré au crayon fuchsia | mis au propre à l'encre, double filet, tampon bleu « FAIT » | graphite et encre blanche sur ardoise |
| **B · Plan d'étage** | murs projetés : doubles tirets | murs en travaux : hachures fuchsia | murs existants : poché noir plein | cyanotype (bleu de Prusse, traits blancs) |
| **C · Relevé de carte** | pointillé au crayon, terra incognita | cartouche à l'encre + fanion fuchsia « vous êtes ici » | cartouche à double filet, lavis, sceau bleu | carte de nuit, encre claire sur papier sombre |

Liens entre batchs : A, trait de crayon et cadenas dessiné ; B, pièces en enfilade, porte fermée = la suivante attend, baie ouverte = passage ; C, chemin pointillé, trait plein pour le chemin parcouru.
Polices d'annotation : A *Architects Daughter*, B *Barlow Condensed* en capitales, C *Cormorant Garamond* italique.

### Deuxième série : le jeu de plateau

Univers des jeux de plateau modernes, plus coloré : chaque epic reçoit une couleur (filet sous l'en-tête, couleur des pièces).

| | Tenu | Courant | Fait | Avancement d'une story | Sombre |
|---|---|---|---|---|---|
| **D · Réseau ferré** (clair, sérieux) | gare en pointillé, voie à poser (traverses seules) | billet de gare avec bande de couleur, gare cerclée de fuchsia, commande sur le talon détachable | gare pleine cochée, voie posée (rails + traverses) | tronçon de cinq wagons, un posé par étape | carte de nuit |
| **E · Plateau de bâtisseurs** (complexe) | tuile face cachée, terrain à bâtir | chantier rayé, ouvrier fuchsia posé sur la tuile, carte « action » pour la commande | bâtiment achevé + jeton bleu | pion de bois sur une piste de 0 à 5 | table en noyer |

Liens : D, voie ferrée (traverses = à poser, rails = posée, cadenas = verrou) ; E, route (bordures en tirets = à tracer, pavée = parcourue). Plateau E sur trame hexagonale.
Polices : D *Zilla Slab*, E *Alegreya SC*.

### Troisième série : le plateau devient le corps de l'epic

D et E restaient des cartes d'interface habillées ; jugées sans intérêt. F et G remplacent le corps de l'epic par un vrai plateau (page *Essais jeu de plateau*, clair seulement pour l'instant).

- **F · Plateau de lignes.** Plateau-carte avec rues et fleuve, bordure à piste de score (0-99, jeton à l'avancement de l'epic). Batch = voie entre deux gares ; story = wagon (slug dedans, cinq rivets = étapes). Wagon imprimé = en cours, wagon plastique posé = soumise ou terminée, voie courte imprimée = batch tenu. Gare cadenassée = verrou, gare cochée = batch soumis, terminus au nom de l'epic. Locomotive fuchsia en tête de la voie courante. Jeton rouge « 2 à débloquer » posé sur le wagon bloqué. Hors batch = billet. Commande = carte « prochain wagon ».
- **G · Tuiles et chantier.** Table en bois, cadre gris à emplacements. Batch tenu = emplacement vide gravé (B2, cadenas, tenu par B1). Batch courant = tuile posée de travers avec ouvrier fuchsia. Batch fait = tuile en place avec blason bleu. Chaque story = une maison numérotée sur la tuile (plan, échafaudage, murs, toit, toit rouge). Fiche du chantier : une ligne par story, un cube de bois par étape (rose écrite, brun design, gris code, violet soumise, or terminée), sceau rouge pour un blocage, ligne « Action » pour la commande. Hors batch = pioche face cachée.

### Retenu : tuiles et chantier, version condensée

Page *Tuiles et chantier* du canevas ; composant `TuilesChantier` (props `epic`, `fond` noyer|tapis, `tuile` illustree|sobre, `ouvert`), interactif.

- Le corps de l'epic ne montre que ses batchs, dans un cadre à emplacements. Plus d'ouvrier, plus de règle du jeu.
- **Tenu** : emplacement vide gravé (id, cadenas, « tenu par », nombre de stories).
- **Courant** : tuile légèrement soulevée, pastille fuchsia « en cours ». **Fait** : tuile en place, drapeau vert.
- Sur chaque tuile : id, nombre de stories, **OQ** (« 3 OQ » en ambre, « OQ ✓ » en vert), blocages (⊖ n). Tuile illustrée : une maison par story qui se construit ; tuile sobre : un cube par story à la couleur de son étape, coche verte pour terminée.
- **Hors batch** : pile face cachée, affichée seulement si elle contient des stories ; au clic, panneau « Hors batch ».
- **Clic sur un batch** : panneau Chantier sous le cadre, relié à la tuile. Contenu : en-tête (état, id complet, nombre de stories, lien vers les OQ du batch), stories avec un cube par étape et coche verte à « terminée », colonne Dépendances et blocages (verrou, chemin de déblocage numéroté), **Action** : proposition complète pour avancer le batch dans un éditeur, modifiable avant de copier (Rétablir / Copier).
- Fonds proposés : noyer (veinage fin, lames) ; tapis de jeu bleu (feutre, couture). Les données OQ des exemples sont fictives.

## 9. Direction validée : l'application sur le tapis de jeu

Page *Application · tapis de jeu* du canevas. Composant unique `EpicPlateau` (props `epic`, `mode` deroulee|repliee, `ouvert`, `chemin` cible|etapes|estompe), interactif : chevron pour replier ou dérouler, clic sur une tuile pour ouvrir son chantier.

**Ce qui ne change pas.** Barre du haut (vues, branche, git), plan à points gris, bandeau de module noir, en-tête d'epic (drapeau, INFIX, anneau d'avancement), pied de page (documents écartés, signalements, tiroir).

**Epic déroulée.** Le corps de l'epic est un tapis de jeu bleu (feutre, couture). Un cadre gris à emplacements contient un emplacement par batch, dans l'ordre :
- tenu : emplacement vide gravé (id, cadenas, « tenu par B1 », nombre de stories) ;
- en cours : tuile crème légèrement soulevée, pastille sarcelle à l'engrenage (grise si l'epic est bloquée) ;
- fait : tuile verte en place, drapeau vert.

Chaque tuile montre son id, un cube par story à la couleur de sa dernière étape franchie (section 11), les OQ (« 3 OQ » ambre ou « OQ ✓ » vert), les RLOQ (puce orange pleine), les blocages (⊖ n), le nombre de stories. La pile bordeaux « Hors batch » suit le cadre, seulement si elle contient des stories.

**Chantier.** Un clic sur une tuile (ou la pile, ou un emplacement vide) ouvre le panneau sous le cadre, relié à la tuile sélectionnée (contour noir) ; l'epic s'élargit et les voisines glissent. Contenu : état, id complet, lien vers les OQ du batch ; stories avec une case par étape (OQ, RLOQ et dépendances posées dans la case qu'elles bloquent) ; « Où le boss s'arrête » (une ligne par story arrêtée, chemin numéroté vers l'autre epic, bouton « Voir sur le plan ») ; **Action** : `boss atw B-…` puis où le boss va s'arrêter, dans un éditeur, modifiable avant copie (Rétablir, Copier). Batch fait : pas d'action. Batch tenu : pas de stories ni d'action, seulement le verrou.

**Epic repliée (240 px).** En-tête, titre, « ⊖ attend BATCHES » si son batch en cours est bloqué par une autre epic (aucune flèche sur le plan), bande de tapis avec une mini-tuile par batch (grise à cadenas, crème à pastille, verte cochée) et la mini-pile hors batch, puis OQ cumulées, RLOQ, blocages et position (« B1/5 »). Sans batch : « aucun batch pour l'instant » et la commande `epicman`.

**Chemin de déblocage.** « Voir sur le plan » : bandeau noir en bas du plan (« Pour débloquer le design file de plugin-split, soumettre : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 », Quitter), numéros 1 et 2 sur les tuiles concernées (aussi sur les mini-tuiles d'une epic repliée), tout le reste estompé à 35 %, la story bloquée surlignée dans le chantier.

À faire ensuite : sombre (le tapis l'est déjà ; reste la barre et le plan), mobile, branches parallèles 1a/1b sur les tuiles. Les nombres d'OQ et les textes d'action des maquettes sont fictifs.

## 10. Design system : les jetons du tapis de jeu

Artefact **Rooftop Design System** : https://claude.ai/artifact/55BLU1ow7qCzDD55Wnifhd (copie dans [`design/design-system/`](../design/design-system/)). Il survit à la fermeture de Claude Design autonome (14 décembre 2026) : c'est un artefact Claude, comme le canevas.

**Contenu.** `tokens.json` (la source : 87 couleurs, 17 styles de texte en trois familles Inter / JetBrains Mono / Alegreya, espacements, rayons, ombres, tailles des pièces, opacités du chemin), `tokens.css` (les mêmes en variables CSS), `README.md` (le brand book : vocabulaire, plan et plateau, états, étapes, chantier, chemin, typographie, icônes, contraste), `components/bundle.css` (classes `rt-` des pièces) et quatorze pièces avec aperçu et mode d'emploi : BandeauModule, EnTeteEpic, EpicRepliee, BandeauChemin, Cadre, Emplacement, Tuile, CubeEtape, Jeton, HorsBatch, Chantier, Drapeau, ModesDuPlan, Arbre.

**Dans le canevas.** Une copie est installée dans `ds/rooftop/` (enregistrée dans `canvas.json`, champ `designSystems`). `EpicPlateau`, `AppPlateau`, `App-Chantier`, `App-Chemin`, `Planche-Plateau` et la nouvelle planche `Palette-Plateau` lient `ds/rooftop/tokens.css` et n'ont plus aucune couleur en dur : tout passe par `var(--jeton)` (dans les SVG, par `style="fill:var(--…)"`). Pour changer une couleur : la modifier dans le design system, puis recopier ses fichiers dans `ds/rooftop/` du canevas ; les planches suivent.

**Limites connues.** Les ombres portent leurs couleurs en dur (`ombre-tuile-en-cours` contient la valeur de `tuile-tranche`) et le grain du tapis encode `tapis-fibre` : à reprendre à la main si ces couleurs changent. Deux paires restent sous le seuil de contraste, gardées des maquettes validées et signalées dans le brand book : texte gravé des emplacements (2:1) et tirets d'epic bloquée (2,5:1).

**Correction en passant.** Sur une tuile, le compte de stories passe à la ligne sous les puces quand « 3 OQ », « ⊖ 1 » et « 4 stories » ne tiennent pas dans 104 px, au lieu de se couper en deux.

## 11. Étapes, arrêts du boss, avancement

**Six étapes par story**, en anglais : ready, brainstorm, design file, code, submitted, done. Une story existe dès qu'elle est écrite ; elle est ready quand ses OQ ont une réponse (definition of ready). Sur une tuile, un cube par story à la couleur de sa dernière étape franchie : gris clair (ready), bleu clair (brainstorm), bleu vif (design file), mauve (code), cube vert (submitted), coche verte ronde (done). Pas ready : cube vide en tirets orange. Submitted et done partagent le même vert : la forme les distingue.

**Une case peut être bloquée** de trois façons, posées dans la case même : des OQ (le boss y répond et continue), des RLOQ — ReLevant Open Questions, que seul un humain tranche — (le boss s'arrête), une dépendance vers un autre batch (le boss s'arrête). Une dépendance vers un batch est levée dès que ce batch est submitted (version beta). Seule compte la dépendance qui bloque une case que le boss va atteindre ; le plan ne dessine aucune flèche de dépendance.

**`boss atw B-PLUGIN-1`** avance chaque story jusqu'à son premier arrêt. Le chantier le montre : cases après l'arrêt estompées, colonne « Où le boss s'arrête », Action qui commence par la commande. Exemple PLUGIN B1 : compound-inventory s'arrête sur ready (1 RLOQ) ; plugin-split fait son brainstorm puis s'arrête sur design file (attend B-TOOLMAN-2) ; shared-skills et marketplace-manifest vont jusqu'au bout.

**L'avancement est calculé, jamais saisi.** Story = étapes franchies / 6 (design file = 50 %). Batch = moyenne de ses stories (tenu 0 %, fait 100 %, stories écartées ignorées). Epic = moyenne de ses batchs ; sans batch, 0 %. Module = moyenne de ses epics. Exemple : quatre batchs dont un seul a toutes ses stories au design file → 50 % / 4 = 12,5 %.

Valeurs des maquettes, recalculées : PLUGIN 7 %, QUALITYGATES 6 %, WORKTREE 50 %, TOOLMAN 11 % (module Toolman 11 %), PIPELINE 0 %, BATCHES 92 %, RTWORKTREES 6 %, TRWG 0 % ; module RB Compound 27 %.

**Cohérence des états.** Tuile, mini-tuile, pied de l'epic repliée et chantier lisent les mêmes stories. Un batch est fait seulement quand toutes ses stories sont done. Une epic bloquée a la pastille grise sur sa tuile, « ⊖ attend BATCHES » sur sa carte, la puce « attend BATCHES » et le cercle barré sur la prochaine case de chaque story dans son chantier. BATCHES n'est plus présentée comme finie : son B4 (unblock-path, code fait) doit passer submitted pour débloquer RTWORKTREES.

## 12. Lisibilité des puces, accent sarcelle, vue Questions

**Accent.** Le fuchsia devient sarcelle (`accent` #0d9488) : pastille ronde à l'engrenage blanc sur une tuile en cours, point d'une mini-tuile en cours, puce « en cours ». Il garde son sens, « on agit ici », et n'est jamais décoratif.

**Puces.** Toujours le nombre d'abord, puis ce qu'il compte : « 3 OQ », « 1 RLOQ » (le mot, jamais un point d'interrogation), « 1 ⊖ ». Même ordre sur la tuile, le pied de l'epic repliée et le chantier.

**Mini-tuiles.** Un clic sur une mini-tuile d'une epic repliée la déroule et ouvre ce batch en un seul geste ; un clic sur le bandeau de l'epic la replie entièrement et ferme le chantier.

**Vue Questions (planche 05).** À gauche, les modules et leurs epics avec leurs puces OQ et RLOQ. À droite, pour l'epic choisie : d'abord les RLOQ, une carte chacune avec la proposition du boss et une réponse à écrire (le boss s'arrête tant qu'elles n'ont pas de réponse, jamais de LGTM en bloc) ; puis les OQ, groupées par batch puis par story, chacune avec la proposition du boss. Un bouton « LGTM sur les N OQ » accepte en un clic toutes les propositions des OQ, jamais celles des RLOQ, et peut s'annuler.

## 13. Priorités sur le plan, et l'Arbre

**L'ordre est la priorité.** On lit le plan comme une page : en haut à gauche, le plus prioritaire, d'abord pour les modules, puis pour les epics dans leur module. On règle cet ordre à la main.

**Un drapeau par module.** Fanion doré (`priorite`, `priorite-bord`), distinct du drapeau vert « fait ». Il se pose sur une epic, un batch ou une story du module ; le reposer ailleurs dans le module le déplace, recliquer dessus le retire. Il apparaît sur la cible (en-tête, tuile, mini-tuile, ligne du chantier ; le batch d'une story au drapeau le porte aussi) et dans le bandeau du module, qui nomme la cible. La priorité reste par module, même quand une epic attend un batch d'un autre module.

**Trois modes, trois touches (planche 06).** Ouvrir (V, par défaut : un clic sur le bandeau d'une epic l'ouvre ou la ferme). Déplacer (M : curseur main, poignée, rang « ‹ n › » au-dessus de chaque epic et module ; on glisse, ou on sélectionne puis ← →). Drapeau (F : outil ponctuel, cibles soulignées au survol en tirets `priorite-bord` sur fond `priorite-pale`, retour au mode d'avant après la pose). Échap annule.

**Ranger par priorité (R, ou « Ranger » dans le bandeau d'un module).** Ordre calculé dans le module : les prérequis de l'epic au drapeau qui sont dans le module ; l'epic au drapeau ; les epics en cours, de la plus avancée à la moins avancée (finir avant de commencer) ; les pas commencées ; les bloquées ; les finies. Sans drapeau, on commence aux epics en cours. Un bilan en bas du plan donne l'ordre, la raison de chaque place et « Annuler », et signale une dépendance vers un autre module. Exemple, drapeau sur plugin-split : PLUGIN, BATCHES (92 %), WORKTREE (50 %), QUALITYGATES (6 %), PIPELINE (pas commencée), RTWORKTREES (attend BATCHES) ; « PLUGIN attend aussi B-TOOLMAN-2, dans le module Toolman ». Drapeau sur RTWORKTREES : BATCHES passe en tête comme prérequis.

**L'Arbre (planche 07), troisième onglet.** Seule vue où l'on dessine des dépendances. Une ligne par epic, sous le bandeau de son module, dans l'ordre du plan ; une colonne par soumission à venir (« Fait », « Maintenant », « +1 »…) : un batch se place dans la première colonne où tout ce dont il dépend est soumis. B-PLUGIN-1, commencé mais qui attend B-TOOLMAN-2, est donc en « +2 », relié à « Maintenant » par des pointillés sarcelle. Liens : trait plein vers le batch suivant de l'epic, tirets rouges coudés avec flèche pour une dépendance bloquante. On choisit en haut le chemin à montrer (vers le drapeau d'un module ou vers un batch cliqué) : il passe en noir avec des étapes numérotées, le reste est estompé, et le bandeau l'écrit : « Chemin vers plugin-split, drapeau de RB Compound : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 → 3 B-PLUGIN-1 ; B-TOOLMAN-1 porte aussi le drapeau de Toolman ».

**Ouvert.** Le critère « plus avancée d'abord » pour les epics en cours ; le sens des colonnes de l'Arbre (quand un batch peut finir, plutôt que quand il peut démarrer) ; proposer ou non de remonter un module dont dépend l'epic au drapeau d'un autre module. Le glisser-déposer n'est pas vérifié dans le canevas ; les flèches de rang et le clavier font le même travail.
