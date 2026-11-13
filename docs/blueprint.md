# Blueprint — spécification de conception

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
Avancement d'une story (pips) : 0 à écrire · 1 sans design · 1,5 design en brouillon · 2 design approuvé, à coder · 3 code fini, à valider puis soumettre · 4 soumise · 5 terminée · −1 écartée.

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
