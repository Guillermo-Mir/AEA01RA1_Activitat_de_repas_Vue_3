<script setup>
import { ref, computed } from 'vue'

// Lista de tasques
const tasques = ref([
  { id: 1, nom: 'Tasca 1', completada: true },
  { id: 2, nom: 'Tasca 2', completada: false },
  { id: 3, nom: 'Tasca 3', completada: false },
  { id: 4, nom: 'Tasca 5', completada: false }
])

// Nova tasca
const novaTasca = ref('')

// Mostrar solo pendents
const mostrarPendents = ref(false)

// Afegir una tasca
const afegirTasca = () => {
  if (novaTasca.value.trim() === "") return

  let nouId = 1
  if (tasques.value.length > 0) {
    const ultimaTasca = tasques.value[tasques.value.length - 1]
    nouId = ultimaTasca.id + 1
  }

  tasques.value.push({
    id: nouId,
    nom: novaTasca.value,
    completada: false
  })

  novaTasca.value = ""
}

// Eliminar una tasca
const eliminarTasca = (id) => {
  tasques.value = tasques.value.filter(t => t.id !== id)
}

// Marcar o desmarcar una tasca
const marcarTasca = (t) => {
  t.completada = !t.completada
}

// Filtrar tasques segons estat
const tasquesFiltrades = computed(() => {
  return mostrarPendents.value
    ? tasques.value.filter(t => !t.completada)
    : tasques.value
})

// Comptar total i pendents
const totalTasques = computed(() => tasques.value.length)
const pendents = computed(() => tasques.value.filter(t => !t.completada).length)
</script>

<template>
  <div style="text-align:center; width:300px; margin:auto">
    <h1>Gestor de Tasques</h1>

    <input 
      v-model="novaTasca" 
      placeholder="Escriu una nova tasca"
    />
    <button @click="afegirTasca">Afegir</button>

    <div>
      <input type="checkbox" v-model="mostrarPendents" />
      <label>Mostra només pendents</label>
    </div>

    <ul style="list-style:none; padding:0">
      <li 
        v-for="tasca in tasquesFiltrades" 
        :key="tasca.id"
        style="margin:6px 0"
      >
        <span :style="{textDecoration: tasca.completada ? 'line-through' : 'none'}">
          {{ tasca.nom }}
        </span>

        <button @click="marcarTasca(tasca)">
          {{ tasca.completada ? 'Desmarcar' : 'Completar' }}
        </button>

        <button @click="eliminarTasca(tasca.id)">
          🗑️
        </button>
      </li>
    </ul>

    <p>Total: {{ totalTasques }} | Pendents: {{ pendents }}</p>
  </div>
</template>

<style scoped></style>
