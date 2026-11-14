Carte d'epic repliée, 240 px : titre, dépendance, bande de tapis à mini-tuiles, pied OQ, blocages et position.

## L'appelant fournit
- l'INFIX, le titre, l'avancement (comme pour l'en-tête) ;
- l'epic qui bloque son batch en cours, s'il y en a une (« attend BATCHES ») : seulement une dépendance bloquante, jamais la liste de toutes ses dépendances ;
- la liste des batchs dans l'ordre, chacun en cours, fait ou tenu ; la présence de stories hors batch ;
- le total des OQ ouvertes, le nombre de RLOQ, le nombre de stories arrêtées par une dépendance, la position « B1/5 » ou un pied libre (« 3 suivis · 2 non mergées ») ;
- la commande à copier quand l'epic n'a pas encore de batch.

## Quand l'utiliser
- Pour toute epic qu'on ne regarde pas : c'est l'état par défaut sur le plan. Un clic sur le chevron la déroule.

## À faire
- Largeur `taille-epic-repliee`. Bande de tapis `tapis`, mini-tuiles de `taille-mini-tuile` : creux `emplacement` au cadenas (tenu), carton `tuile-en-cours` au point `accent` (en cours, `accent-bloque` si l'epic est bloquée), carton `tuile-faite` à la coche (fait), mini-pile `hors-batch` en dernier.
- Pied : puce OQ (style `puce`), puce de blocage si besoin, position en `encre-muette`.
- Sans batch : « aucun batch pour l'instant » en `tapis-texte`, puis la bande de commande `surface-creuse` en mono.

## À éviter
- Ne mets pas le titre dans l'en-tête d'une carte repliée : il passe sous l'INFIX, sur sa propre ligne.
- Pas de flèche de dépendance sur le plan, vers aucune epic : la ligne « ⊖ attend BATCHES » (`.rt-attend`, `blocage-texte`) suffit.
