# Liste de tâches (Vue 3)

**Auteur :** DhiaEddine Rezgani — L2DSI1

Application pour ajouter, terminer et supprimer des tâches (`<script setup>`, `ref`, `computed`, `v-for`, `v-model`). Aucune API : les tâches disparaissent au rechargement.

## Commandes
```bash
npm install
npm run dev      # http://localhost:5173
npm run build
```

## Fonctionnalités
- Ajout d'une tâche (vide ou espaces refusés, champ vidé après l'ajout) ; identifiant unique par tâche
- Case à cocher « Terminée » (texte barré), réactivable
- Suppression d'une seule tâche
- Compteur de tâches non terminées ; message « Aucune tâche pour le moment. »
- Bonus : filtres Toutes / En cours / Terminées

## Rôles de `v-for`, `:key` et `computed`
- **`v-for`** répète le `<li>` pour chaque tâche du tableau réactif `taches`.
- **`:key="t.id"`** donne à chaque tâche une identité unique : Vue sait quelle ligne a changé, a été ajoutée ou supprimée et ne met à jour que celle-ci (une suppression n'affecte pas les autres lignes).
- **`computed`** : `nonTerminees` est recalculé automatiquement dès que le tableau ou l'état d'une tâche change, sans code supplémentaire ; il en va de même pour la liste filtrée.

## Captures
`captures/01-plusieurs-taches.png` et `captures/02-tache-terminee.png`.
