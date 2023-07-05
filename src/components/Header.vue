<script setup>
defineProps(['nombre'])
</script>

<template>
  <header class="contenedor user">
    <div class="profile">
      <i class="fa-solid fa-user"></i>
    </div>
    <h3>Hola, {{ nombre }}</h3>
  </header>
</template>

<style lang="scss" scoped>
.user {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
  font-size: 2rem;
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