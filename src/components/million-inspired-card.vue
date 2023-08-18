<script setup lang="ts">
import { useMouseInElement } from '@vueuse/core';
import { reactive, ref, watchEffect } from 'vue';

const cardRef = ref<HTMLDivElement | null>(null);
const mouse = reactive(useMouseInElement(cardRef));

const rotateX = ref('0deg');
const rotateY = ref('0deg');

const MAX_ROTATION = 10;

watchEffect(() => {
  const middleY = mouse.elementHeight / 2;
  const middleX = mouse.elementWidth / 2;

  const offsetY = !mouse.isOutside
    ? ((mouse.elementY - middleY) / middleY) * MAX_ROTATION
    : 0;

  const offsetX = !mouse.isOutside
    ? ((mouse.elementX - middleX) / middleX) * MAX_ROTATION
    : 0;

  rotateX.value = -1 * offsetX + 'deg';
  rotateY.value = 1 * offsetY + 'deg';
});
</script>

<template>
  <div class="container">
    <div
      ref="cardRef"
      class="card language-css"
      :class="[{ card_transition: mouse.isOutside }]"
      tabindex="0"
    >
      <!-- <pre>{{ JSON.stringify(mouse, null, 2) }}</pre> -->
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 500px;
  height: 300px;
  color: gray;
  background: hsl(222, 45%, 7%);
  padding: 2rem;
  border-radius: 1rem;

  position: relative;

  transition: transform 0.2s ease-out;
  transform-style: preserve-3d;
  transform: perspective(5000px) rotateY(v-bind(rotateX))
    rotateX(v-bind(rotateY));
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
