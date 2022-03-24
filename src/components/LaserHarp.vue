<script setup>
import { computed,  ref } from 'vue';
import HarpLine from './HarpLine.vue';

const touching = ref(false);
const touchPosition = ref({ x: 0, y: 0 });

const pointer = ref();
const pointerRect = computed(() => {
  const size = 40;

  return {
    top: Math.round(touchPosition.value.y - size / 2),
    right: Math.round(touchPosition.value.x + size / 2),
    bottom: Math.round(touchPosition.value.y + size / 2),
    left: Math.round(touchPosition.value.x - size / 2),
  }
});

const setTouchPosition = (event) => {
  const evt = (typeof event.originalEvent === 'undefined') ? event : event.originalEvent;
  const touch = evt.touches[0] || evt.changedTouches[0];
  touchPosition.value = { x: touch.pageX, y: touch.pageY };
}

const handleTouchStart = (event) => {
  touching.value = true;
  setTouchPosition(event);
};

const handleTouchMove = (event) => {
  setTouchPosition(event);
  
};

const handleTouchEnd = (event) => {
  touching.value = false;
  setTouchPosition(event);
};
</script>

<template>
  <div class="wrapper" @touchstart="handleTouchStart" @touchmove="handleTouchMove" @touchend="handleTouchEnd">
    <div class="body"></div>
    <div class="line-wrapper">
      <HarpLine class="line" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :touching="touching" :pointerRect="pointerRect"></HarpLine>
    </div>
    <div v-show="touching" ref="pointer" class="pointer" :style="{ top: `${touchPosition.y}px`, left: `${touchPosition.x}px` }">ここ</div>
  </div>
</template>

<style scoped>
.wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #000;
}

.body {
  background: #D3D3D3;
  color: #333;
  width: 20%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1;

  display: grid;
  place-items: center;
}

.line-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.line {
  --color: rgb(58, 246, 58);
  --color1: yellow;
  --color2: lime;
  --color3: green;
  --color4: darkgreen;

  width: 40%;
  height: 4px;
  background: var(--color);
  box-shadow: 
    0 0 10px 0 var(--color1),
    0 0 20px 0 var(--color2),
    0 0 40px 0 var(--color3),
    0 0 80px 0 var(--color4);
  position: absolute;
}

.line[data-active='true'] {
  --color: red;
}

.line:nth-of-type(1),
.line:nth-of-type(2) {
  top: 30%;
}

.line:nth-of-type(3),
.line:nth-of-type(4) {
  top: 50%;
}

.line:nth-of-type(5),
.line:nth-of-type(6) {
  top: 70%;
}

.line:nth-of-type(odd) {
  left: 0;
}

.line:nth-of-type(even) {
  right: 0;
}

.pointer {
  background: #fff;
  transform: translate(-50%, -50%);
  width: 40px;
  height: 40px;
  border-radius: 9999px;
  position: absolute;
}
</style>
