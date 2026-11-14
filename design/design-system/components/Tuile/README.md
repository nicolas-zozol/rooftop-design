Tuile de carton posée dans un emplacement : un batch en cours ou fait, avec ses stories en cubes.

## L'appelant fournit
- l'id court (B1), l'état (en cours ou fait) et si l'epic est bloquée ;
- une étape par story (0 à écrire, 1 écrite, 1,5 design en brouillon, 2 design, 3 code, 4 soumise, 5 terminée ; les stories écartées ne font pas de cube) ;
- le nombre d'OQ ouvertes, le nombre de stories bloquées, le nombre de stories ;
- l'état sélectionné (chantier ouvert dessus) et, sur le chemin, son numéro d'étape.

## Quand l'utiliser
- Pour chaque batch en cours ou fait d'une epic déroulée. C'est un bouton : il ouvre et ferme le chantier.

## À faire
- En cours : `tuile-en-cours`, soulevée de 2 px (`ombre-tuile-en-cours`), pastille `accent` au marteau. Epic bloquée : pastille `accent-bloque`, tuile à plat.
- Fait : `tuile-faite`, à plat (`ombre-tuile-posee`), pastille `surface` au drapeau `fait`.
- Sélectionnée : soulevée de 5 px (`ombre-tuile-selection`), anneau `selection` de 2,5 px décalé de 3 px.
- Id en `id-tuile`, puces en `puce`, compte en `tuile-muette`. `title` complet : « B1 · en cours · 4 stories · 3 OQ à répondre ».

## À éviter
- Pas de texte de story sur la tuile : les slugs sont dans le chantier.
- Ne retire jamais la puce OQ : « OQ ✓ » dit qu'il n'y en a pas.
