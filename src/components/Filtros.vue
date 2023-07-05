<script setup>
defineProps(['filtro'])
defineEmits(['update:filtro'])
</script>
<template>
  <div class="filtros contenedor">
    <form>
      <div class="campo">
        <label for="filtro">Filtrar por categoría</label>
        <select id="filtro" :value="filtro" @input="$emit('update:filtro', $event.target.value)">
            <option value="">Todos</option>
            <option value="ahorro">Ahorro</option>
            <option value="comida">Comida</option>
            <option value="casa">Casa</option>
            <option value="gastos">Gastos Varios</option>
            <option value="ocio">Ocio</option>
            <option value="salud">Salud</option>
            <option value="suscripciones">Suscripciones</option>
          </select>
      </div>
    </form>
  </div>
</template>

<style lang="scss" scoped>
.filtros {
  margin-top: 1rem;
  padding: 5rem;
  background-color: var(--gris-oscuro);
  border-radius: 1rem;
  @media (max-width: 580px) {
    padding: 3rem;
  }
}
.filtros .campo {
  display: grid;
  gap: 1rem;
}
.filtros label {
  font-size: 1.5rem;
  opacity: 0.5;
}
</style>
