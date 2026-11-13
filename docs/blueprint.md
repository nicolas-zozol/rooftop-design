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

## 8. Ouvert

- Le traitement graphique des batchs et le mode sombre sont jugés trop « classiques » : une évolution purement graphique (style architecte / papier) est en cours, en pistes séparées, à côté de l'existant.
