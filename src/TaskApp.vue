<script setup>
import { ref, computed } from 'vue'

// Lista de tasques
const tasques = ref([
  { id: 1, nom: 'Tasca 1', completada: true },
  { id: 2, nom: 'Tasca 2', completada: false },
  { id: 3, nom: 'Tasca 3', completada: false },
  { id: 4, nom: 'Tasca 4', completada: false }
])

const novaTasca = ref('')
const mostrarPendents = ref(false)

const afegirTasca = () => {
  if (novaTasca.value.trim() === '') return
  let nouId = 1
  if (tasques.value.length > 0) {
    nouId = tasques.value[tasques.value.length - 1].id + 1
  }
  tasques.value.push({
    id: nouId,
    nom: novaTasca.value,
    completada: false
  })
  novaTasca.value = ''
}

const eliminarTasca = (id) => {
  tasques.value = tasques.value.filter(t => t.id !== id)
}

const marcarTasca = (t) => {
  t.completada = !t.completada
}

const tasquesFiltrades = computed(() => {
  return mostrarPendents.value
    ? tasques.value.filter(t => !t.completada)
    : tasques.value
})

const totalTasques = computed(() => tasques.value.length)
const pendents = computed(() => tasques.value.filter(t => !t.completada).length)
</script>

<template>
  <div class="contenidor">
    <h1>Gestor de Tasques</h1>

    <div class="nova-tasca">
      <input 
        v-model="novaTasca" 
        placeholder="Escriu una nova tasca"
      />
      <button @click="afegirTasca" class="btn btn-principal">Afegir</button>
    </div>

    <div class="filtre">
      <label>
        <input type="checkbox" v-model="mostrarPendents" />
        Mostra només pendents
      </label>
    </div>

    <ul class="llista-tasques">
      <li 
        v-for="tasca in tasquesFiltrades" 
        :key="tasca.id"
        :class="{ completada: tasca.completada }"
      >
        <div class="tasca-text">
          {{ tasca.nom }}
        </div>
        <div class="accions">
          <button @click="marcarTasca(tasca)" class="btn btn-secundari">
            {{ tasca.completada ? 'Desmarcar' : 'Completar' }}
          </button>
          <button @click="eliminarTasca(tasca.id)" class="btn btn-eliminar">
            🗑️
          </button>
        </div>
      </li>
    </ul>

    <p class="resum">
      Total: {{ totalTasques }} | Pendents: {{ pendents }}
    </p>
  </div>
</template>

<style scoped>
/* === Paleta ===
   Fondo: #f8f6f2 (crema claro)
   Tonos arena: #d9cbbd
   Terracota: #c27b48
   Marrón oscuro: #5a4636
   Gris cálido: #8b8178
*/

body {
  background-color: #f8f6f2;
}

.contenidor {
  width: 430px;
  margin: 60px auto;
  background-color: #fffaf5;
  border-radius: 20px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
  padding: 35px;
  text-align: center;
  font-family: 'Inter', 'Segoe UI', sans-serif;
}

h1 {
  color: #5a4636;
  margin-bottom: 30px;
  font-size: 26px;
  letter-spacing: 0.5px;
}

/* === Input + botón === */
.nova-tasca {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

.nova-tasca input {
  flex: 1;
  padding: 12px;
  font-size: 16px;
  border: 2px solid #d9cbbd;
  border-radius: 12px;
  background-color: #fdfaf7;
  color: #5a4636;
  outline: none;
  transition: border-color 0.2s;
}

.nova-tasca input:focus {
  border-color: #c27b48;
}

.btn {
  border: none;
  border-radius: 12px;
  padding: 10px 18px;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.2s;
  font-size: 15px;
}

.btn:hover {
  transform: scale(1.03);
}

.btn-principal {
  background-color: #c27b48;
  color: #fffaf5;
}

.btn-principal:hover {
  background-color: #a86a3e;
}

.btn-secundari {
  background-color: #8b8178;
  color: #fffaf5;
}

.btn-secundari:hover {
  background-color: #766b61;
}

.btn-eliminar {
  background-color: #bfa58a;
  color: #fffaf5;
}

.btn-eliminar:hover {
  background-color: #a88e73;
}

/* === Filtro === */
.filtre {
  margin-bottom: 25px;
  color: #5a4636;
  font-size: 15px;
}

.filtre input {
  margin-right: 8px;
}

/* === Lista de tareas === */
.llista-tasques {
  list-style: none;
  padding: 0;
  margin: 0;
}

.llista-tasques li {
  background-color: #fefdfb;
  border: 1px solid #e3dcd2;
  border-radius: 16px;
  margin: 12px 0;
  padding: 16px 18px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: background-color 0.2s, transform 0.1s;
}

.llista-tasques li:hover {
  background-color: #f8f3ed;
  transform: scale(1.01);
}

.tasca-text {
  font-size: 17px;
  color: #4b3b2d;
  text-align: left;
  flex: 1;
}

.llista-tasques li.completada .tasca-text {
  text-decoration: line-through;
  color: #a89988;
}

.accions {
  display: flex;
  gap: 10px;
}

/* === Resumen === */
.resum {
  margin-top: 25px;
  color: #5a4636;
  font-size: 15px;
}
</style>
