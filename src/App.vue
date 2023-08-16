<script setup lang="ts">
/* no-prettier */
</script>

<template>
  <div class="container">
    <div class="card language-css" tabindex="0"></div>
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
