<script setup lang="ts">
import TiltCard from './components/3d-tilt-card.vue'
import AstroCard from './components/astro-card.vue'
import MillionCard from './components/million-card.vue'
</script>

<template>
  <div class="w-screen h-screen flex justify-around items-center">
    <AstroCard />
    <TiltCard />
    <MillionCard />
  </div>
</template>

<style scoped></style>
