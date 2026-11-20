<script setup>
import { ref, computed } from 'vue'

const nouvelleTache = ref('')   // texte du champ de saisie
const taches = ref([])          // tableau réactif : { id, libelle, terminee }
let prochainId = 1              // identifiant unique de la prochaine tâche
const filtre = ref('toutes')    // bonus : toutes | en cours | terminees

function ajouter() {
  const libelle = nouvelleTache.value.trim()
  if (libelle === '') return // refuse le vide et les espaces seuls
  taches.value.push({ id: prochainId++, libelle, terminee: false })
  nouvelleTache.value = ''   // vide le champ
}

function supprimer(id) {
  taches.value = taches.value.filter((t) => t.id !== id)
}

// Compteur : se recalcule automatiquement
const nonTerminees = computed(() => taches.value.filter((t) => !t.terminee).length)

// Bonus : liste affichée selon le filtre choisi
const tachesAffichees = computed(() => {
  if (filtre.value === 'en cours') return taches.value.filter((t) => !t.terminee)
  if (filtre.value === 'terminees') return taches.value.filter((t) => t.terminee)
  return taches.value
})
</script>

<template>
  <main class="appli">
    <h1>Liste de tâches</h1>

    <form class="saisie" @submit.prevent="ajouter">
      <input v-model="nouvelleTache" type="text" placeholder="Nom de la tâche" aria-label="Nom de la tâche" />
      <button type="submit">Ajouter</button>
    </form>

    <p class="compteur">
      {{ nonTerminees }} tâche(s) non terminée(s)
    </p>

    <div v-if="taches.length" class="filtres">
      <button v-for="f in ['toutes', 'en cours', 'terminees']" :key="f"
              :class="{ actif: filtre === f }" @click="filtre = f">
        {{ f === 'terminees' ? 'Terminées' : f.charAt(0).toUpperCase() + f.slice(1) }}
      </button>
    </div>

    <p v-if="taches.length === 0" class="vide">Aucune tâche pour le moment.</p>

    <ul v-else>
      <li v-for="t in tachesAffichees" :key="t.id">
        <label>
          <input v-model="t.terminee" type="checkbox" />
          <span :class="{ barre: t.terminee }">{{ t.libelle }}</span>
        </label>
        <button class="suppr" @click="supprimer(t.id)">Supprimer</button>
      </li>
    </ul>
  </main>
</template>

<style>
* { box-sizing: border-box; }
body { margin: 0; font-family: 'Segoe UI', Arial, sans-serif; background: #eef1f6; color: #1f2937; }
.appli { max-width: 520px; margin: 30px auto; padding: 20px; background: white; border-radius: 12px; box-shadow: 0 2px 8px rgba(0,0,0,.08); }
h1 { margin-top: 0; }
.saisie { display: flex; gap: 8px; }
.saisie input { flex: 1; padding: 10px; border: 1px solid #9ca3af; border-radius: 6px; font-size: 16px; }
button { padding: 9px 14px; border: none; border-radius: 6px; background: #1d4ed8; color: white; font-weight: 600; cursor: pointer; font-size: 15px; }
.compteur { font-weight: 700; color: #1d4ed8; }
.filtres { display: flex; gap: 6px; margin-bottom: 10px; }
.filtres button { background: #e5e7eb; color: #1f2937; }
.filtres .actif { background: #1d4ed8; color: white; }
ul { list-style: none; padding: 0; margin: 0; }
li { display: flex; justify-content: space-between; align-items: center; gap: 8px; padding: 10px 0; border-bottom: 1px solid #e5e7eb; }
label { display: flex; align-items: center; gap: 10px; cursor: pointer; flex: 1; }
.barre { text-decoration: line-through; color: #6b7280; }
.suppr { background: #b91c1c; }
.vide { color: #6b7280; text-align: center; }
</style>
