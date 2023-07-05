<script setup>
import { computed } from "vue";
import CircleProgress from "vue3-circle-progress"
import "vue3-circle-progress/dist/circle-progress.css"
import { formatearCantidad } from '../helpers'

const props = defineProps({
  presupuesto: {
    type: Number,
    required: true
  },
  disponible: {
    type: Number,
    required: true
  },
  gastado: {
    type: Number,
    required: true
  },
})
defineEmits(['reset-app'])

const porcentaje = computed(() => {
  return parseInt(((props.presupuesto - props.disponible) / (props.presupuesto)) * 100)
})
</script>

<template>
  <div class="control contenedor">
    <section class="total sombra">
      <div>
        <p>
          <small>Prespuesto:</small>
          <span>{{ formatearCantidad(presupuesto) }}</span>
        </p>
      </div>
      <div class="contenedor-grafico">
        <div class="porcentaje">{{ porcentaje }}%</div>
        <CircleProgress
          :percent="porcentaje"
          :viewport="true"
          :size="110"
          :border-width="18"
          :border-bg-width="18"
          fill-color="#6583CC"
          empty-color="#3e4450"
        />
      </div>
    </section>
    <div class="sub-total">
      <div class="disponible sombra">
        <p>
          <i class="fa-solid fa-sack-dollar icon-disponible"></i>
          <small>Disponible:</small>
          <span>{{ formatearCantidad(disponible) }}</span>
        </p>
      </div>
      <div class="gastado sombra">
        <p>
          <i class="fa-solid fa-basket-shopping icon-gastado"></i>
          <small>Gastos:</small>
          <span>{{ formatearCantidad(gastado) }}</span>
        </p>
      </div>
    </div>
    <button class="button sombra" @click="$emit('reset-app')">
      Reiniciar
    </button>
  </div>
</template>

<style lang="scss" scoped>
.control {
  display: grid;
  gap: 1rem;
  .total {
    background-color: var(--gris-oscuro);
    border-radius: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 5rem;
    @media (max-width: 580px) {
      padding: 3rem;
      flex-direction: column;
      gap: 3rem;
      div:first-child {
        order: 2;
      }
    }
  }
  .sub-total {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    @media (max-width: 580px) {
      grid-template-columns: 1fr;
    }
    .disponible, .gastado {
      background-color: var(--gris-oscuro);
      border-radius: 1rem;
      padding: 5rem;
      position: relative;
      @media (max-width: 580px) {
        padding: 3rem;
      }
    }
  }
  p {
    display: grid;
    .icon-disponible,
    .icon-gastado {
      position: absolute;
      top: 3rem;
      right: 3rem;
      height: 4rem;
      width: 4rem;
      border-radius: 0.5rem;
      display: grid;
      place-content: center;
      background-color: var(--gris);
      @media (max-width: 580px) {
        right: 1rem;
        top: 1rem;
      }
    }
    .icon-disponible {
      // background-color: rgba(34, 197, 94, 0.3);
      color: #22c55e;
    }
    .icon-gastado {
      // background-color: rgb(239, 68, 68, 0.3);
      color: #ef4444,
    }
    span {
      font-size: 3rem;
      font-weight: 700;
    }
    small {
      font-size: 1.5rem;
      opacity: 0.5;
    }
  }

}
.contenedor-presupuesto {
  width: 100%;
}
.contenedor-presupuesto span {
  font-weight: 900;
  color: var(--azul);
}
.contenedor-grafico {
  position: relative;
}
.porcentaje {
  position: absolute;
  margin: auto;
  top: calc(50% - 1rem);
  left: 0;
  right: 0;
  text-align: center;
  z-index: 100;
  font-size: 1.5rem;
  font-weight: 900;
}
</style>