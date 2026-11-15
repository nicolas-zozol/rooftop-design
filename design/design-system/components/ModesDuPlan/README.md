Barre des modes du plan et ce qui va avec : Ouvrir, Déplacer, l'outil Drapeau, Ranger par priorité, la pastille de rang et le bilan d'un rangement.

## L'appelant fournit
- le mode courant (`aria-pressed` sur le bon bouton) ; en Drapeau, le mode d'avant reste enfoncé à côté ;
- pour chaque epic et chaque module en Déplacer : son rang, de 1 au nombre de pièces ;
- après un rangement : le nouvel ordre et la raison de chaque place.

## Quand l'utiliser
- En haut à gauche du plan, au-dessus des modules. Chaque bouton montre sa touche en `.rt-kbd` : Ouvrir V, Déplacer M, Drapeau F, Ranger R ; Échap annule le drapeau ou termine le déplacement.
- **Ouvrir** : un clic sur le bandeau d'une epic l'ouvre ou la ferme.
- **Déplacer** : curseur main (`.rt-saisir`), poignée à la place de l'icône, `.rt-rang` « ◀ rang ▶ » au-dessus de chaque epic et de chaque module. On glisse, ou on sélectionne puis ← → (↑ ↓ sautent une rangée).
- **Drapeau** : outil ponctuel. Les cibles (`.rt-cible`, `.rt-cible-fond`) se soulignent en tirets `priorite-bord` au survol ; après la pose, on revient au mode d'avant.
- **Ranger** (barre : tous les modules ; bandeau : ce module) range les epics : prérequis de l'epic au drapeau dans le module, l'epic au drapeau, les epics en cours de la plus avancée à la moins avancée, les pas commencées, les bloquées, les finies.

## À faire
- Après une pose ou un rangement, le bilan `.rt-bilan` en bas du plan : l'ordre numéroté, la raison de chaque place, « Annuler », et la dépendance vers un autre module s'il y en a une.
- Lis l'ordre comme une page : en haut à gauche, le plus prioritaire.

## À éviter
- Pas de glisser sans alternative : les flèches de `.rt-rang` et les touches restent là.
- Ranger ne touche jamais au drapeau ni aux autres modules.
