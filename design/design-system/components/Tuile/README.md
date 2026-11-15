Tuile de carton posée dans un emplacement : un batch en cours ou fait, avec ses stories en cubes.

## L'appelant fournit
- l'id court (B1), l'état (en cours ou fait) et si l'epic est bloquée ;
- pour chaque story, sa dernière étape franchie : pas ready, ready, brainstorm, design file, code, submitted ou done (les stories écartées ne font pas de cube) ;
- le nombre d'OQ ouvertes (RLOQ comprises), le nombre de RLOQ, le nombre de stories arrêtées par une dépendance, le nombre de stories ;
- l'état sélectionné (chantier ouvert dessus) et, sur le chemin, son numéro d'étape.

## Quand l'utiliser
- Pour chaque batch en cours ou fait d'une epic déroulée. C'est un bouton : il ouvre et ferme le chantier.

## À faire
- En cours : `tuile-en-cours`, soulevée de 2 px (`ombre-tuile-en-cours`), pastille `accent` à l'engrenage. Epic bloquée : pastille `accent-bloque`, tuile à plat.
- Fait : `tuile-faite`, à plat (`ombre-tuile-posee`), pastille `surface` au drapeau `fait`.
- Sélectionnée : soulevée de 5 px (`ombre-tuile-selection`), anneau `selection` de 2,5 px décalé de 3 px.
- Id en `id-tuile`, puces en `puce` (OQ, puis RLOQ pleine `rloq`, puis ⊖), compte en `tuile-muette`. `title` complet : « B1 · en cours · 4 stories · 3 OQ à répondre, dont 1 RLOQ ».

## À éviter
- Pas de texte de story sur la tuile : les slugs sont dans le chantier.
- Pas de cube pour une étape en cours : seule la dernière étape franchie compte ; pas ready, le cube est vide en tirets orange.
- Ne retire jamais la puce OQ : « OQ ✓ » dit qu'il n'y en a pas.
