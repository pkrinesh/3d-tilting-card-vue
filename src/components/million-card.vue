<script setup lang="ts">
import { useMouseInElement } from '@vueuse/core';
import { computed, reactive, ref } from 'vue';

const cardRef = ref<HTMLDivElement | null>(null);
const mouse = reactive(useMouseInElement(cardRef));
const MAX_ROTATION = 10;

const cardTransform = computed(() => {
  const offsetX =
    (mouse.elementX / mouse.elementWidth) * MAX_ROTATION - MAX_ROTATION / 2;
  const offsetY =
    (mouse.elementY / mouse.elementHeight) * MAX_ROTATION - MAX_ROTATION / 2;

  const rX = -1 * offsetX + 'deg';
  const rY = 1 * offsetY + 'deg';

  return mouse.isOutside
    ? ''
    : `perspective(5000px) rotateY(${rX}) rotateX(${rY})`;
});
</script>

<template>
  <div class="container">
    <div
      ref="cardRef"
      tabindex="0"
      class="card language-css"
      :style="{
        // transform: cardTransform,
        // transition: 'transform 0.25s ease-out',
      }"
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
  transform: v-bind(cardTransform);
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
