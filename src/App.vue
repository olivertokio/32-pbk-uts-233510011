<script setup>
import HelloWorld from './components/HelloWorld.vue'

</script>

<template>
  <div>

    <!-- <a href="#" target="_blank">
      <img src="./assets/1.jpg" class="logo" alt="Vite logo" />
    </a> -->

  </div>
  <HelloWorld msg="Vite + Vue" />
</template>

<style scoped>

.logo {
  border-radius: 150px;
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
