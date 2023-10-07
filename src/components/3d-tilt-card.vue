<script setup lang="ts">
import {
  // useMouse,
  useMouseInElement,
} from '@vueuse/core'
import { computed, reactive, ref, watchEffect } from 'vue'

const cardRef = ref<HTMLDivElement | null>(null)
const mouse = reactive(useMouseInElement(cardRef, { target: window }))
// const mousePos = reactive(useMouse({ type: 'client' }))

// const middleY = window.innerHeight / 2
// const middleX = window.innerWidth / 2

const middle = computed(() => ({
  x: mouse.elementPositionX + mouse.elementWidth / 2,
  y: mouse.elementPositionY + mouse.elementHeight / 2,
}))

const rotateX = ref('0deg')
const rotateY = ref('0deg')

watchEffect(() => {
  const offsetX = ((mouse.x - middle.value.x) / middle.value.x) * 20
  const offsetY = ((mouse.y - middle.value.y) / middle.value.y) * 20

  // const offsetX = ((mousePos.x - middleX) / middleX) * 20;
  // const offsetY = ((mousePos.y - middleY) / middleY) * 20;

  rotateX.value = offsetX + 'deg'
  rotateY.value = -1 * offsetY + 'deg'
})
</script>

<template>
  <div class="">
    <div ref="cardRef" class="card flex justify-center items-center" tabindex="0">
      <div>Follow the mouse (Window)</div>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 400px;
  height: 200px;
  color: gray;
  background: hsl(222, 45%, 7%);
  padding: 2rem;
  border-radius: 1rem;

  position: relative;

  transform-style: preserve-3d;
  transform: perspective(5000px) rotateY(v-bind(rotateX)) rotateX(v-bind(rotateY));
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
