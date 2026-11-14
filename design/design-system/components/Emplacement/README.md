Emplacement vide et gravé : le batch existe mais attend la soumission d'un autre.

## L'appelant fournit
- l'id court du batch (B2), le batch qui le tient (« tenu par B1 »), son nombre de stories ;
- son numéro d'étape quand il est sur le chemin de déblocage.

## Quand l'utiliser
- Pour tout batch tenu. Un clic ouvre le chantier en mode tenu : pas de stories, pas d'Action, la phrase « Ce batch attend la soumission de B1. »

## À faire
- Creux `emplacement` avec `ombre-emplacement`, texte gravé `emplacement-grave` : id en `id-emplacement`, ligne et compte en `legende`.
- Sur le chemin : pastille `.rt-etape-num` (`selection`, bord `surface`) en haut à gauche ; hors du chemin : `opacite-estompe`.

## À éviter
- Ne remplis pas un emplacement tenu avec une tuile grise : le creux dit l'attente.
- Le texte gravé est à 2:1 : ne s'en sert pas pour une information qu'on ne trouve nulle part ailleurs (le chantier et le `title` la répètent).
