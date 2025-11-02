<script setup>
import { ref, computed } from 'vue'

//Lista reactiva de tasques
const tasques = ref([
  { id: 1, nom: 'Tasca 1', completada: true },
  { id: 2, nom: 'Tasca 2', completada: false },
  { id: 3, nom: 'Tasca 3', completada: false },
  { id: 4, nom: 'Tasca 5', completada: false }
])

//Campo para nueva tasca
const novaTasca = ref('')

//Checkbox: mostrar solo pendents
const mostrarPendents = ref(false)

//Función para afegir nova tasca
const afegirTasca = () => {
  // Si el texto está vacío, no hacemos nada
  if (novaTasca.value.trim() === "") return

  //Si NO hay tareas aún, el id empezará en 1
  let nouId = 1

  //Si ya existen tareas, el nuevo id será el último + 1
  if (tasques.value.length > 0) {
    const ultimaTasca = tasques.value[tasques.value.length - 1] // Último elemento del array
    nouId = ultimaTasca.id + 1
  }

  //Añadimos la nueva tarea al array
  tasques.value.push({
    id: nouId,
    text: novaTasca.value
  })

  //Limpiamos la entrada de texto
  novaTasca.value = ""
}

//Función para eliminar una tasca
const eliminarTasca = (id) => {
  tasques.value = tasques.value.filter(t => t.id !== id)
}

//Función para marcar/desmarcar tasca
const marcarTasca = (t) => {
  t.completada = !t.completada
}

//Mostrar según estado (pendents o totes)
const tasquesFiltrades = computed(() => {
  return mostrarPendents.value
    ? tasques.value.filter(t => !t.completada)
    : tasques.value
})

//Contadores
const totalTasques = computed(() => tasques.value.length)
const pendents = computed(() => tasques.value.filter(t => !t.completada).length)
</script>

<template>
  <div style="text-align:center; width:300px; margin:auto">

    <h1>Gestor de Tasques</h1>

    <!--Input + botón -->
    <input 
      v-model="novaTasca" 
      placeholder="Escriu una nova tasca"
    />
    <button @click="afegirTasca">Afegir</button>

    <!--Checkbox para filtrar -->
    <div>
      <input type="checkbox" v-model="mostrarPendents" />
      <label>Mostra només pendents</label>
    </div>

    <!--Lista de tasques -->
    <ul style="list-style:none; padding:0">
      <li 
        v-for="tasca in tasquesFiltrades" 
        :key="tasca.id"
        style="margin:6px 0"
      >
        <!-- Nombre tachado si completada -->
        <span :style="{textDecoration: tasca.completada ? 'line-through' : 'none'}">
          {{ tasca.nom }}
        </span>

        <!-- Botón completar / desmarcar -->
        <button @click="marcarTasca(tasca)">
          {{ tasca.completada ? 'Desmarcar' : 'Completar' }}
        </button>

        <!-- Botón eliminar -->
        <button @click="eliminarTasca(tasca.id)">
          🗑️
        </button>
      </li>
    </ul>

    <!--Resumen -->
    <p>Total: {{ totalTasques }} | Pendents: {{ pendents }}</p>
  </div>
</template>



<style scoped></style>
