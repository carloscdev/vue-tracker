<template>
  <div class="alerta">
    <slot />
  </div>
</template>
<style scoped>
.alerta {
  position: fixed;
  bottom: 5rem;
  left: 0;
  right: 0;
  width: 50rem;
  max-width: 95%;
  margin: 0 auto;
  padding: 1.5rem 3rem;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.6rem;
  text-align: center;
  border-radius: 1rem;
  background-color: #ef4444;
}
</style>
