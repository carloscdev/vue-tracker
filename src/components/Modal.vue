<script setup>
import { ref, computed } from 'vue';
import CerrarModal from '../assets/img/cerrar.svg'
import Alerta from './Alerta.vue'

const error = ref('')

const emit = defineEmits(['ocultar-modal', 'guardar-gasto', 'eliminar-gasto', 'update:nombre', 'update:cantidad', 'update:categoria'])
const props = defineProps({
  modal: {
    type: Object,
    required: true
  },
  nombre: {
    type: String,
    required: true
  },
  cantidad: {
    type: [String, Number],
    required: true
  },
  categoria: {
    type: String,
    required: true
  },
  disponible: {
    type: Number,
    required: true
  },
  id: {
    type: [String, null],
    required: true
  }
})

const old = props.cantidad

const agregarGasto = () => {
  // Validar que no haya campos vacíos
  const { nombre, cantidad, categoria, disponible, id } = props
  if ([nombre, cantidad, categoria].includes("")) {
    error.value = "Todos los campos son obligatorios"
    setTimeout(() => {
      error.value = ""
    }, 3000)
    return
  }

  // Validar la cantidad
  if (cantidad <= 0) {
    error.value = 'Cantidad no válida'
    setTimeout(() => {
      error.value = ""
    }, 3000)
    return
  }

  // Validar que el usuario no gaste más de lo disponible
  if (id) {
    if (cantidad > old + disponible) {
      error.value = 'Haz excedido el presupuesto'
      setTimeout(() => {
        error.value = ""
      }, 3000)
      return
    }
  } else {
    if (cantidad > disponible) {
      error.value = 'Haz excedido el presupuesto'
      setTimeout(() => {
        error.value = ""
      }, 3000)
      return
    }
  }

  emit('guardar-gasto')
}

const textoBoton = computed(() => props.id ? 'Editar' : 'Añadir')
</script>

<template>
  <div class="modal" @click="$emit('ocultar-modal')">
    <div @click.stop class="contenedor contenedor-formulario sombra" :class="modal.animar ? 'animar' : ''">
      <div class="cerrar-modal" @click="$emit('ocultar-modal')">
        <i class="fa-solid fa-xmark"></i>
      </div>
      <form class="nuevo-gasto" @submit.prevent="agregarGasto">
        <legend>{{ textoBoton }} Gasto</legend>
        <Alerta v-if="error">
          {{  error  }}
        </Alerta>
        <div class="campo">
          <label for="nombre">Título:</label>
          <input
            type="text"
            id="nombre"
            placeholder="Ingresa el título del gasto"
            :value="nombre"
            @input="$emit('update:nombre', $event.target.value)"
          >
        </div>
        <div class="campo">
          <label for="cantidad">Monto:</label>
          <input
            type="number"
            id="cantidad"
            placeholder="Ingresa un monto, ej. 300"
            min="0"
            :value="cantidad"
            @input="$emit('update:cantidad', +$event.target.value)"
          >
        </div>
        <div class="campo">
          <label for="categoria">Categoria:</label>
          <select id="categoria" :value="categoria" @input="$emit('update:categoria', $event.target.value)">
            <option value="">Seleccione una categoría</option>
            <option value="ahorro">Ahorro</option>
            <option value="comida">Comida</option>
            <option value="casa">Casa</option>
            <option value="gastos">Gastos Varios</option>
            <option value="ocio">Ocio</option>
            <option value="salud">Salud</option>
            <option value="suscripciones">Suscripciones</option>
          </select>
        </div>
        <input type="submit" class="button" :value="textoBoton">
        <button v-if="id" class="button danger" @click="$emit('eliminar-gasto', id)">
          Eliminar
        </button>
      </form>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.modal {
  position: fixed;
  background-color: rgba(30, 30, 35, 0.95);
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 200;
  padding-top: 15rem;
  overflow-y: hidden !important;
}
.cerrar-modal {
  position: absolute;
  right: 1.5rem;
  top: 1.5rem;
  font-size: 2rem;
  background-color: var(--gris-oscuro);
  padding: 1rem;
  border-radius: 0.5rem;
  cursor: pointer;
  width: 4rem;
  height: 4rem;
  display: grid;
  place-content: center;
  @media (max-width: 580px) {
    bottom: 1rem;
    right: 1rem;
  }
}
.contenedor-formulario {
  transition-property: all;
  transition-duration: 300ms;
  transition-timing-function: ease-in;
  opacity: 0;
}
.contenedor-formulario.animar {
  opacity: 1;
}
.nuevo-gasto {
  background-color: var(--gris-oscuro);
  border-radius: 1rem;
  padding: 5rem;
  display: grid;
  gap: 2rem;
  position: relative;
  @media (max-width: 580px) {
    padding: 3rem;
  }
}
.nuevo-gasto legend {
  font-weight: 700;
  font-size: 2rem;
}
.campo {
  display: grid;
  gap: 1rem;
}
.nuevo-gasto label {
  font-size: 1.5rem;
  opacity: 0.5;
}
</style>