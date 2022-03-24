<script setup>
import { onMounted } from 'vue';

import LaserHarp from './components/LaserHarp.vue'

onMounted(() => {
  window.addEventListener('contextmenu', (e) => {
    e.preventDefault();
  });
});
</script>

<template>
  <LaserHarp />
</template>

<style>
body {
  overscroll-behavior: none;
}
</style>
