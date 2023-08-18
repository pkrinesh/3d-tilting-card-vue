<script setup lang="ts">
// inspired from LearnVue
// https://github.com/LearnVueCo/tutorial-code/blob/main/apps/astro/src/tutorials/3d-card/Card.vue
import { useMouseInElement } from '@vueuse/core';
import { computed, reactive, ref } from 'vue';

const cardRef = ref<HTMLDivElement | null>(null);
const mouse = reactive(useMouseInElement(cardRef));

const MAX_ROTATION = 12;

const cardTransform = computed(() => {
  const rX = (
    (mouse.elementY / mouse.elementHeight) * MAX_ROTATION -
    MAX_ROTATION / 2
  ).toFixed(2); // handles x-axis

  const rY = (
    (mouse.elementX / mouse.elementWidth) * MAX_ROTATION -
    MAX_ROTATION / 2
  ).toFixed(2); // handles y-axis

  return mouse.isOutside
    ? ''
    : `perspective(${
        mouse.elementWidth
      }px) rotateX(${rX}deg) rotateY(${-rY}deg)`;
});
</script>

<template>
  <div class="container">
    <div
      ref="cardRef"
      class="card language-css"
      :style="{
        transform: cardTransform,
        transition: 'transform 0.25s ease-out',
      }"
      tabindex="0"
    ></div>
  </div>
</template>

<style scoped>
.card {
  width: 500px;
  height: 300px;
  color: gray;
  background: #aa076b;
  background: -webkit-linear-gradient(to top, #61045f, #aa076b);
  background: linear-gradient(to top, #61045f, #aa076b);
  padding: 2rem;
  border-radius: 1rem;

  position: relative;
}
</style>
