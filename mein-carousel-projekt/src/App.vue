<script setup>
import EmblaCarousel from './components/EmblaCarousel.vue'
</script>

<template>
  <main class="p-6 max-w-4xl mx-auto">
    <h1 class="text-3xl font-bold text-center mb-6">A11Y DeepDive Carousel</h1>
    <EmblaCarousel/>
  </main>
</template>


<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
