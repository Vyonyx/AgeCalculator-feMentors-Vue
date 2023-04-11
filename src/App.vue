<script setup lang="ts">
import AgeCalculatorCard from './components/AgeCalculatorCard.vue'
</script>

<template>
  <main>
    <AgeCalculatorCard />
  </main>
</template>

<style scoped lang="scss">
@import url('./style.css');

main {
  background: var(--off-white);
  height: 100vh;
  padding: 1rem;
}
</style>
