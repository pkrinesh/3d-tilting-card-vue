<script setup lang="ts">
import { useMouseInElement } from '@vueuse/core'
import { computed, reactive, ref } from 'vue'

const cardRef = ref<HTMLDivElement | null>(null)
const mouse = reactive(useMouseInElement(cardRef))
const MAX_ROTATION = 12

const cardTransform = computed(() => {
  const offsetX = (mouse.elementY / mouse.elementHeight) * MAX_ROTATION - MAX_ROTATION / 2
  const offsetY = (mouse.elementX / mouse.elementWidth) * MAX_ROTATION - MAX_ROTATION / 2

  const rX = -1 * offsetX + 'deg'
  const rY = 1 * offsetY + 'deg'

  return mouse.isOutside ? '' : `perspective(${mouse.elementWidth}px) rotateX(${rX}) rotateY(${rY})`
})
</script>

<template>
  <div class="">
    <div
      ref="cardRef"
      tabindex="0"
      class="card flex justify-center items-center shadow-lg bg-gradient-to-b from-zinc-700 to-zinc-800"
      :style="{
        // transform: cardTransform,
        // transition: 'transform 0.25s ease-out',
      }"
    >
      <!-- <pre>{{ JSON.stringify(mouse, null, 2) }}</pre> -->
      <p>Million.js inspired card.</p>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 400px;
  height: 200px;
  color: white;
  /* background: #641ae6; */
  /* background: #434343; */
  padding: 2rem;
  border-radius: 1rem;

  position: relative;

  transition: transform 0.2s ease-out;
  transform-style: preserve-3d;
  transform: v-bind(cardTransform);
}
</style>
