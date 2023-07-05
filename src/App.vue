<script setup>
import { ref, reactive, watch, computed, onMounted } from 'vue';
import { generarId } from './helpers'
import Presupuesto from './components/Presupuesto.vue'
import ControlPresupuesto from './components/ControlPresupuesto.vue';
import Modal from './components/Modal.vue';
import Gasto from './components/Gasto.vue';
import Filtros from './components/Filtros.vue';
import Empty from './components/Empty.vue';
import Header from './components/Header.vue';

const modal = reactive({
  mostrar: false,
  animar: false
})
const nombre = ref('')
const presupuesto = ref(0)
const disponible = ref(0)
const gastado = ref(0)
const filtro = ref('')

const gasto = reactive({
  nombre: '',
  cantidad: 0,
  categoria: '',
  id: null,
  fecha: Date.now(),
})

const gastos = ref([])

watch(gastos, () => {
  const totalGastado = gastos.value.reduce((total, gasto) => total + gasto.cantidad, 0)
  gastado.value = totalGastado
  disponible.value = presupuesto.value - totalGastado

  localStorage.setItem('gastos', JSON.stringify(gastos.value))
}, { deep: true })

watch(presupuesto, () => {
  localStorage.setItem('presupuesto', presupuesto.value)
})

watch(nombre, () => {
  localStorage.setItem('nombre', nombre.value)
})

onMounted(() => {
  const presupuestoStorage = localStorage.getItem('presupuesto')
  if (presupuestoStorage) {
    presupuesto.value = Number(presupuestoStorage)
    disponible.value = Number(presupuestoStorage)

    const gastosStorage = localStorage.getItem('gastos')
    if(gastosStorage) {
      gastos.value = JSON.parse(gastosStorage)
    }
  }
  const usuario = localStorage.getItem('nombre')
  if (usuario) {
    nombre.value = usuario
  } else {
    nombre.value = 'bienvenido !'
  }
})

const definirPresupuesto = (datos) => {
  const { cantidad, usuario } = datos
  presupuesto.value = cantidad
  disponible.value = cantidad
  nombre.value = usuario
}

const mostrarModal = () => {
  modal.mostrar = true
  document.body.classList.add('fijar');
  setTimeout(() => {
    modal.animar = true
  }, 300)
}
const ocultarModal = () => {
  modal.animar = false
  document.body.classList.remove('fijar');
  setTimeout(() => {
    modal.mostrar = false
    Object.assign(gasto, {
      nombre: '',
      cantidad: '',
      categoria: '',
      id: null,
      fecha: Date.now(),
    })
  }, 500)
}

const guardarGasto = () => {
  if (gasto.id) {
    const { id } = gasto
    const i = gastos.value.findIndex((item) => item.id === id)
    gastos.value[i] = {
      ...gasto
    }
  } else {
    gastos.value.push({
      ...gasto,
      id: generarId()
    })
  }
  ocultarModal()
}

const seleccionarGasto = (id) => {
  const gastoEditar = gastos.value.filter(item => item.id === id)[0]
  Object.assign(gasto, gastoEditar)
  mostrarModal()
}

const elminarGasto = (id) => {
  gastos.value = gastos.value.filter((item) => item.id !== id)
  ocultarModal()
}

const gastosFiltrados = computed(() => {
  if (filtro.value) {
    return gastos.value.filter(item => item.categoria === filtro.value)
  }
  return gastos.value
})

const resetApp = () => {
  gastos.value = []
  presupuesto.value = 0
  disponible.value = 0
}
</script>

<template>
  <div>
    <Transition name="slide-fade" mode="out-in">
      <Presupuesto
        v-if="!presupuesto"
        @definir-presupuesto="definirPresupuesto"
      />
      <main v-else>
        <Header :nombre="nombre" />
        <ControlPresupuesto
            :presupuesto="presupuesto"
            :disponible="disponible"
            :gastado="gastado"
            @reset-app="resetApp"
          />
        <Filtros v-model:filtro="filtro" />
        <div v-if="gastosFiltrados.length > 0" class="listado-gastos contenedor">
          <h2>Transacciones Recientes</h2>
          <Gasto
            v-for="gasto in gastosFiltrados"
            :key="gasto.id"
            :gasto="gasto"
            @seleccionar-gasto="seleccionarGasto"
          />
        </div>
        <Empty v-else />
        <div class="crear-gasto sombra" @click="mostrarModal">
          <i class="fa-solid fa-plus"></i>
        </div>
        <Modal
          v-if="modal.mostrar"
          @ocultar-modal="ocultarModal"
          @guardar-gasto="guardarGasto"
          @eliminar-gasto="elminarGasto"
          :modal="modal"
          :disponible="disponible"
          :id="gasto.id"
          v-model:nombre="gasto.nombre"
          v-model:cantidad="gasto.cantidad"
          v-model:categoria="gasto.categoria"
        />
      </main>
    </Transition>
  </div>
</template>

<style lang="scss">
:root {
  --rosa: #6583CC;
  --blanco: #fff;
  --gris-claro: #f5f5f5;
  --gris: #3e4450;
  --gris-oscuro: #303642;
  --negro: #1E1E23;
}
html {
  font-size: 62.5%;
  box-sizing: border-box;
}
*,
*::before,
*::after {
  box-sizing: inherit;
  margin: 0;
  padding: 0;
}
body {
  font-size: 1.6rem;
  font-family: "Lato", sans-serif;
  background-color: var(--negro);
  color: var(--blanco);
  padding: 10rem 0;
}
.button {
  background: linear-gradient(135deg, #6583CC, #5524F5);
  border: none;
  padding: 1.5rem 1rem;
  color: var(--blanco);
  font-weight: 700;
  width: 100%;
  border-radius: 0.5rem;
  height: 5rem;
  transition: background-color 300ms ease;
  cursor: pointer;
  &:hover {
    opacity: 0.9;
  }
  &.danger {
    background: #ef4444;
    border: 1px solid #ef4444;
  }
}
.fijar {
  overflow: hidden;
  height: 100vh;
}
input[type="number"],
input[type="text"],
select {
  width: 100%;
  background-color: var(--gris);
  padding: 1.5rem 1rem;
  color: var(--blanco);
  border-radius: 0.5rem;
  height: 5rem;
  padding: 1rem;
  border: none;
  border: 1px solid;
  border-color: transparent transparent var(--blanco) transparent;
  &:focus {
    outline: none;
    border-color: var(--rosa);
  }
}
.contenedor {
  width: 95%;
  max-width: 70rem;
  margin: 0 auto;
}
.sombra {
  box-shadow: 0px 10px 15px -3px rgba(0,0,0,0.1);
}
.crear-gasto {
  position: fixed;
  bottom: 5rem;
  right: 5rem;
  background: linear-gradient(135deg, #6583CC, #5524F5);
  height: 5rem;
  width: 5rem;
  display: grid;
  place-content: center;
  font-size: 3rem;
  border-radius: 0.5rem;
  cursor: pointer;
  @media (max-width: 580px) {
    bottom: 1rem;
    right: 1rem;
  }
}
.listado-gastos {
  margin-top: 3rem;
  display: grid;
  gap: 1rem;
  h2 {
    font-weight: 700;
  }
}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  /* transform: translateY(20px); */
  opacity: 0;
}
</style>
