# rooftop-design

Maquettes et spécification de l'écran **Blueprint** de Rooftop (app de bureau Tauri, lecture seule, graphe React Flow des modules, epics, batchs et stories).

## Où regarder

> **Direction retenue : tuiles sobres sur tapis de jeu.** Écrans : page *Application · tapis de jeu* du canevas (`AppPlateau`, `App-Chantier`, `App-Chemin`, `Planche-Plateau`, composant `EpicPlateau`). Les autres pages gardent l'historique des pistes.

- **Le canevas vivant** (artefact Claude de type Design) : https://claude.ai/artifact/3EwPxSWQuHuvURMgwKGjyR
  Pages : *Application · tapis de jeu* (ouverture), *Palette et tailles* (jetons du système), puis l'historique : *Maquettes*, *Planche des composants*, *Pistes batchs*, *Essais jeu de plateau*, *Tuiles et chantier*.
- **Le design system** : https://claude.ai/artifact/55BLU1ow7qCzDD55Wnifhd — jetons, brand book et onze pièces du tapis de jeu. Copie dans [`design/design-system/`](design/design-system/), installée dans le canevas sous `ds/rooftop/`.
- **La spec écrite** : [`docs/blueprint.md`](docs/blueprint.md) — hiérarchie, états, chemin de déblocage, contrat backend, mobile, jetons ; section 10 pour le design system.
- **Les sources du canevas** : [`design/canvas/`](design/canvas/) — `canvas.json` (index des planches, notes) et un fichier `.dc.html` par planche ou composant.

## Contenu de `design/canvas/`

| Fichier | Rôle |
|---|---|
| `Main.dc.html` | 01 · Plan à l'ouverture (props `theme`, `insp`, `vue`) |
| `Deroulement.dc.html` | 02 · Epic repliée, puis déroulée |
| `Inspecteur.dc.html`, `InspecteurEtapes.dc.html` | 03a/03b · Story bloquée : la cible, puis les étapes sur le plan |
| `Hierarchie.dc.html` | 04 · Quatre niveaux, quatre signatures |
| `Mobile.dc.html` | 05 · Mobile, plus tard : ce que la conception prévoit |
| `CarteEpic.dc.html` | Composant : carte d'epic repliée (bandeau module fusionné possible) |
| `CarteBatch.dc.html` | Composant : batch ouvert ou en pastille, étape de chemin |
| `EpicCompound.dc.html` | Composant : epic PLUGIN déroulée |
| `PanneauInspecteur.dc.html` | Composant : inspecteur sur `plugin-split` bloquée |
| `CheminBranches.dc.html` | Chemin à deux branches (1a, 1b, 2) |
| `Planche-*.dc.html` | Planches des états de chaque composant |
| `Palette-Plateau.dc.html` | **Retenu** · jetons du design system : couleurs, typographie, espacements, rayons, relief, tout en `var(--jeton)` |
| `ds/rooftop/` | Copie du design system dans le canevas : `tokens.json`, `tokens.css`, `components/bundle.css` |
| `Palette.dc.html` | Ancienne palette, avant le tapis de jeu |
| `*Sombre.dc.html`, `*-sombre.dc.html` | Jumeaux sombres : importent la planche claire avec `theme="sombre"` |
| `Piste-A.dc.html`, `BatchCrayon.dc.html` | Piste graphique A · Planche à dessin : crayon → encre (page *Pistes batchs*) |
| `Piste-B.dc.html`, `BatchPiece.dc.html` | Piste graphique B · Plan d'étage : le batch est une pièce, les murs disent l'état |
| `Piste-C.dc.html`, `BatchCarte.dc.html` | Piste graphique C · Relevé de carte : itinéraire, fanion « vous êtes ici » |
| `Piste-D.dc.html`, `BatchGare.dc.html` | Piste jeu de plateau D · Réseau ferré : gares, tronçons de cinq wagons |
| `Piste-E.dc.html`, `BatchTuile.dc.html` | Piste jeu de plateau E · Plateau de bâtisseurs : tuiles, ouvrier, pions sur piste 0-5 |
| `Piste-F.dc.html`, `PlateauLignes.dc.html` | Essai F · Plateau de lignes : voies de wagons entre gares, piste de score au bord (page *Essais jeu de plateau*) |
| `Piste-G.dc.html`, `PlateauTuiles.dc.html` | Essai G · Tuiles et chantier : cadre à emplacements, maisons par story, fiche à cubes |
| `EpicPlateau.dc.html` | **Retenu** · carte d'epic sur le tapis : repliée (mini-tuiles) ou déroulée (cadre à tuiles, Hors batch, chantier au clic), étapes du chemin |
| `AppPlateau.dc.html`, `App-Chantier.dc.html`, `App-Chemin.dc.html` | **Retenu** · l'application : plan à l'ouverture, chantier B1 ouvert, chemin de déblocage sur le plan |
| `Planche-Plateau.dc.html` | **Retenu** · planche : les huit epics repliées, panneaux fait, tenu, hors batch |
| `Chantier-1.dc.html`, `Chantier-2.dc.html`, `TuilesChantier.dc.html` | Tuiles et chantier condensé, interactif : tuiles de l'epic, panneau Chantier au clic, action éditable ; fonds noyer et tapis (page *Tuiles et chantier*) |

Les fichiers `.dc.html` sont le format du canevas Claude Design : balisage `<x-dc>` avec trous `{{…}}`, boucles `<sc-for>`, conditions `<sc-if>`, imports `<dc-import>`, et un script `DCLogic` dont `renderVals()` calcule les valeurs. Ils s'ouvrent dans le canevas, pas directement dans un navigateur.

## Changer une couleur

1. Modifier le jeton dans le design system (sa page, ou `design/design-system/tokens.json` puis republication).
2. Recopier `tokens.json`, `tokens.css` et `components/bundle.css` du système vers `ds/rooftop/` du canevas.
3. Les planches retenues (`EpicPlateau`, `AppPlateau`, `App-*`, `Planche-Plateau`, `Palette-Plateau`) suivent : elles n'ont plus de couleur en dur.

## Contenu de `design/design-system/`

Les fichiers de l'artefact, tels que publiés : `design-system.json` (index), `tokens.json` (la source), `tokens.css` (généré), `README.md` (le brand book), `components/bundle.css` et un dossier par pièce (`README.md` + `preview.html`), plus `components/Cover/` (la couverture). Pour recréer le système ailleurs, republier ces fichiers sous `project/` dans un artefact de type Design System.

## Historique

Chaque étape de conception est un commit : on peut revenir à n'importe quel état du canevas en republiant les fichiers de `design/canvas/` à ce commit.
