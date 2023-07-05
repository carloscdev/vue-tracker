<script setup>
defineProps(['nombre'])
defineEmits(['reset-app'])
</script>

<template>
  <header class="contenedor user">
    <nav>
      <div class="profile">
        <i class="fa-solid fa-user"></i>
      </div>
      <h3>Hola, {{ nombre }}</h3>
    </nav>
    <button class="button danger sombra" @click="$emit('reset-app')" title="Reiniciar">
      <i class="fa-solid fa-right-from-bracket"></i>
    </button>
  </header>
</template>

<style lang="scss" scoped>
.user {
  margin-bottom: 1rem;
  display: grid;
  grid-template-columns: 1fr 5rem;
  nav {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1rem;
    font-size: 2rem;
  }
  .profile {
  background-color: var(--gris-oscuro);
  width: 5rem;
  height: 5rem;
  display: grid;
  place-content: center;
  border-radius: 0.5rem;
}
}
</style>