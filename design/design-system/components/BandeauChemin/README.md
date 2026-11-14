Bandeau noir en haut du plan qui dit dans quel ordre soumettre pour débloquer une story.

## L'appelant fournit
- la story à débloquer (slug) ;
- la suite ordonnée des batchs à soumettre, chacun avec son id complet (B-TOOLMAN-1) ;
- l'action « Quitter le chemin ».

## Quand l'utiliser
- Seulement après « Voir sur le plan » dans le chantier d'une story bloquée. Le reste du plan passe à `opacite-estompe` et les tuiles du chemin portent leur numéro (`.rt-etape-num`).

## À faire
- Fond `inverse`, texte `texte-inverse`, numéros en pastilles `texte-inverse` sur `inverse`, ids en mono, bouton en `inverse-2`.
- Une seule phrase, qui nomme la case bloquée : « Pour débloquer le design file de plugin-split, soumettre : 1 B-TOOLMAN-1 → 2 B-TOOLMAN-2 ».

## À éviter
- Pas plus d'un bandeau à la fois. Pas de fermeture automatique : on quitte le chemin par le bouton.
