# rooftop-design

Maquettes et spécification de l'écran **Blueprint** de Rooftop (app de bureau Tauri, lecture seule, graphe React Flow des modules, epics, batchs et stories).

## Où regarder

- **Le canevas vivant** (Claude Design) : https://claude.ai/artifact/3EwPxSWQuHuvURMgwKGjyR
  Trois pages : *Maquettes*, *Planche des composants*, *Palette et tailles*.
- **La spec écrite** : [`docs/blueprint.md`](docs/blueprint.md) — hiérarchie, états, chemin de déblocage, contrat backend, mobile, jetons.
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
| `Palette.dc.html` | Couleurs et tailles |
| `*Sombre.dc.html`, `*-sombre.dc.html` | Jumeaux sombres : importent la planche claire avec `theme="sombre"` |
| `Piste-A.dc.html`, `BatchCrayon.dc.html` | Piste graphique A · Planche à dessin : crayon → encre (page *Pistes batchs*) |
| `Piste-B.dc.html`, `BatchPiece.dc.html` | Piste graphique B · Plan d'étage : le batch est une pièce, les murs disent l'état |
| `Piste-C.dc.html`, `BatchCarte.dc.html` | Piste graphique C · Relevé de carte : itinéraire, fanion « vous êtes ici » |
| `Piste-D.dc.html`, `BatchGare.dc.html` | Piste jeu de plateau D · Réseau ferré : gares, tronçons de cinq wagons |
| `Piste-E.dc.html`, `BatchTuile.dc.html` | Piste jeu de plateau E · Plateau de bâtisseurs : tuiles, ouvrier, pions sur piste 0-5 |
| `Piste-F.dc.html`, `PlateauLignes.dc.html` | Essai F · Plateau de lignes : voies de wagons entre gares, piste de score au bord (page *Essais jeu de plateau*) |
| `Piste-G.dc.html`, `PlateauTuiles.dc.html` | Essai G · Tuiles et chantier : cadre à emplacements, maisons par story, fiche à cubes |

Les fichiers `.dc.html` sont le format du canevas Claude Design : balisage `<x-dc>` avec trous `{{…}}`, boucles `<sc-for>`, conditions `<sc-if>`, imports `<dc-import>`, et un script `DCLogic` dont `renderVals()` calcule les valeurs. Ils s'ouvrent dans le canevas, pas directement dans un navigateur.

## Historique

Chaque étape de conception est un commit : on peut revenir à n'importe quel état du canevas en republiant les fichiers de `design/canvas/` à ce commit.
