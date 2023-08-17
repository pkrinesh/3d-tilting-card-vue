<script setup lang="ts">
import { ref, watchEffect } from 'vue';
import { useMouse } from '@vueuse/core';

const cardRef = ref<HTMLDivElement | null>(null);
const mousePos = useMouse({ type: 'client' });

watchEffect(() => {
  rotateElement(mousePos);
});

function rotateElement(mouse: typeof mousePos) {
  if (!cardRef.value) return;

  // get mouse position
  const x = mouse.x.value;
  const y = mouse.y.value;

  // find the middle
  const middleX = window.innerWidth / 2;
  const middleY = window.innerHeight / 2;

  // get offset from middle as a percentage
  // and tone it down a little
  const offsetX = ((x - middleX) / middleX) * 45;
  const offsetY = ((y - middleY) / middleY) * 45;

  // set rotation
  cardRef.value?.style.setProperty('--rotateX', offsetX + 'deg');
  cardRef.value?.style.setProperty('--rotateY', -1 * offsetY + 'deg');
}
</script>

<template>
  <div class="container">
    <div ref="cardRef" class="card language-css" tabindex="0"></div>
  </div>
</template>

<style scoped>
.card {
  --rotateX: 0deg;
  --rotateY: 0deg;

  width: 500px;
  height: 300px;
  font-size: 3rem;
  font-weight: bold;
  background: hsl(222, 45%, 7%);
  padding: 2rem;
  border-radius: 1rem;

  position: relative;

  transform-style: preserve-3d;
  transform: perspective(5000px) rotateY(var(--rotateX)) rotateX(var(--rotateY));
}

.card::before,
.card::after {
  content: '';
  position: absolute;
  border-radius: inherit;
}

/* shadow */
.card::before {
  inset: 0.75rem;
  border-radius: inherit;
  background: black;
  z-index: -1;
  transform: translateZ(-50px);
  filter: blur(15px);
  opacity: 0.5;
}

/* gradient thingy */
.card::after {
  z-index: -2;
  inset: -1.2rem;

  background: #aa076b;
  background: -webkit-linear-gradient(to top, #61045f, #aa076b);
  background: linear-gradient(to top, #61045f, #aa076b);

  transform: translateZ(-50px);
}
</style>
