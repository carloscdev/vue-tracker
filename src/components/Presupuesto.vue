<script setup>
import { ref } from 'vue';
import Alerta from './Alerta.vue'

const presupuesto = ref(0)
const nombre = ref('')
const error = ref('')

const emit = defineEmits(['definir-presupuesto'])

const definirPresupuesto = () => {
  if (presupuesto.value <= 0 || isNaN(presupuesto.value)) {
    error.value = 'Presupuesto no válido'

    setTimeout(() => {
      error.value = ''
    }, 3000)
    return
  }
  if (!nombre.value) {
    error.value = 'Ingresa tu nombre'
    setTimeout(() => {
      error.value = ''
    }, 3000)
    return
  }
  emit('definir-presupuesto', { cantidad: presupuesto.value, usuario: nombre.value })
}
</script>

<template>
  <section class="contenedor presupuesto sombra">
    <form @submit.prevent="definirPresupuesto">
      <legend>Control de gastos</legend>
      <Alerta v-if="error">
        {{  error  }}
      </Alerta>
      <div class="campo">
        <label for="nombre">Nombre:</label>
        <input
          id="nombre"
          class="nuevo-presupuesto"
          type="text"
          placeholder="Ingresa tu nombre"
          min="0"
          v-model="nombre"
        >
      </div>
      <div class="campo">
        <label for="nuevo-presupuesto">Ingresa tu presupuesto</label>
        <input
          id="nuevo-presupuesto"
          class="nuevo-presupuesto"
          type="number"
          step="any"
          placeholder="Presupuesto:"
          min="0"
          v-model.number="presupuesto"
        >
      </div>
      <input type="submit" class="button" value="Continuar">
    </form>
</section>
</template>

<style lang="scss" scoped>
.presupuesto {
  background-color: var(--gris-oscuro);
  border-radius: 1rem;
  padding: 5rem;
  form {
    display: grid;
    gap: 2rem;
  }
}
.campo {
  display: grid;
  gap: 1rem;
}
.presupuesto label {
  font-size: 1.5rem;
  opacity: 0.5;
}
legend {
  font-weight: 700;
  font-size: 2rem;
}
</style>