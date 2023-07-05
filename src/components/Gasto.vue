<script setup>
import { formatearCantidad, formatearFecha } from '../helpers'
const props = defineProps({
  gasto: {
    type: Object,
    required: true
  }
})

const diccionarioIconos = {
  ahorro : 'fa-solid fa-chart-line blue',
  comida : 'fa-solid fa-apple-whole pink',
  casa : 'fa-solid fa-house orange',
  gastos : 'fa-solid fa-money-bill green',
  ocio : 'fa-solid fa-mug-saucer yellow',
  salud : 'fa-solid fa-briefcase-medical red',
  suscripciones : 'fa-solid fa-circle-play purple'
}

defineEmits(['seleccionar-gasto'])

</script>

<template>
  <div class="sombra gasto" @click="$emit('seleccionar-gasto', gasto.id)">
    <div class="contenido">
      <i :class="diccionarioIconos[gasto.categoria]"></i>
      <div class="detalles">
        <p class="nombre">{{ gasto.nombre }}</p>

        <p class="fecha">
          <span>{{ formatearFecha(gasto.fecha) }}</span>
        </p>
      </div>
    </div>
    <p class="cantidad">- {{ formatearCantidad(gasto.cantidad) }}</p>
  </div>
</template>

<style lang="scss" scoped>
.gasto {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: var(--gris-oscuro);
  border-radius: 1rem;
  padding: 3rem;
  cursor: pointer;
  animation: fadeIn 300ms ease-in;
  @media (max-width: 580px) {
    padding: 1.5rem;
  }
  .fa-solid {
    background-color: var(--gris);
    padding: 1.5rem;
    border-radius: 0.5rem;
    font-size: 2rem;
    &.blue {
      color: #3b82f6;
    }
    &.pink {
      color: #ec4899;
    }
    &.orange {
      color: #f97316;
    }
    &.green {
      color: #22c55e;
    }
    &.yellow {
      color: #eab308;
    }
    &.red {
      color: #ef4444;
    }
    &.purple {
      color: #a855f7;
    }
  }
  .contenido {
    display: flex;
    gap: 2rem;
    align-items: center;
  }

  .nombre {
    font-size: 2.4rem;
    font-weight: 700;
  }
  &:hover {
    .nombre {
      color: var(--rosa);
    }
  }
  .fecha {
    font-size: 1.3rem;
    opacity: .7;
  }
  .cantidad {
    font-size: 2rem;
    font-weight: 700;
  }
}
@keyframes fadeIn {
  from { opacity:0; }
  to { opacity:1; }
}
</style>