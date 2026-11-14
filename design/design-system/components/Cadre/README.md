Tapis de jeu et cadre à emplacements : le corps d'une epic déroulée, un emplacement par batch.

## L'appelant fournit
- la liste ordonnée des batchs de l'epic (B1, B2…), chacun en cours, fait ou tenu par un autre batch ;
- pour chaque batch posé : ses stories et leur étape, ses OQ ouvertes, ses stories bloquées ;
- les stories hors batch, s'il y en a (la pile se pose à droite du cadre).

## Quand l'utiliser
- Dans toute epic déroulée. Le cadre ne montre que les batchs de cette epic.

## À faire
- Tapis : `.rt-tapis` (`tapis`, couture `tapis-couture`, grain `tapis-fibre`), marge `pad-tapis`.
- Cadre : `.rt-cadre` (`cadre`, `ombre-cadre`, `radius-cadre`, `pad-cadre`), emplacements de `taille-tuile` espacés de `gap-tuile`.
- Pile Hors batch à `gap-hors-batch` du cadre, alignée en haut.
- Un clic sur une tuile ouvre le chantier sous le cadre ; un second clic le referme.

## À éviter
- Pas de cadre vide : une epic sans batch reste repliée avec sa commande.
- Pas de retour à la ligne dans le cadre : il défile avec le plan.
