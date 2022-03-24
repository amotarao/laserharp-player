<script setup>
import { computed,  ref } from 'vue';
import HarpLine from './HarpLine.vue';

const mode = ref('full');

const touching = ref(false);
const touchPosition = ref({ x: 0, y: 0 });

// const pointer = ref();
const pointerRect = computed(() => {
  const width = 16;
  const height = 64;

  return {
    top: Math.round(touchPosition.value.y - height / 2),
    right: Math.round(touchPosition.value.x + width / 2),
    bottom: Math.round(touchPosition.value.y + height / 2),
    left: Math.round(touchPosition.value.x - width / 2),
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

const audio = ref();
const handleClickPlay = () => {
  audio.value.play();
}
</script>

<template>
  <div class="wrapper" @touchstart="handleTouchStart" @touchmove="handleTouchMove" @touchend="handleTouchEnd">
    <div class="body">
      <img src="@/assets/body.svg" width="200" height="480" />
    </div>
    <div class="line-wrapper">
      <HarpLine class="line" :mode="mode" position="left" audioSrc="https://dotup.org/uploda/dotup.org2756920.mp3" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :mode="mode" position="right" audioSrc="https://dotup.org/uploda/dotup.org2756923.mp3" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :mode="mode" position="left" audioSrc="https://dotup.org/uploda/dotup.org2756924.mp3" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :mode="mode" position="right" audioSrc="https://dotup.org/uploda/dotup.org2756925.mp3" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :mode="mode" position="left" audioSrc="https://dotup.org/uploda/dotup.org2756920.mp3" :touching="touching" :pointerRect="pointerRect"></HarpLine>
      <HarpLine class="line" :mode="mode" position="right" audioSrc="https://dotup.org/uploda/dotup.org2756923.mp3" :touching="touching" :pointerRect="pointerRect"></HarpLine>
    </div>
    <!-- <div v-show="touching" ref="pointer" class="pointer" :style="{ top: `${touchPosition.y}px`, left: `${touchPosition.x}px` }"></div> -->
    <div class="controller">
      <input type="radio" name="mode" v-model="mode" value="full" />
      <input type="radio" name="mode" v-model="mode" value="touch" />
      <button @click="handleClickPlay">音声が流れないときに1回押す</button>
      <audio ref="audio" src="/audio-muted.mp3" />
    </div>
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
  color: #333;
  width: 70%;
  height: 100%;
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1;
  display: grid;
  place-items: center;
}

.body img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  object-position: center bottom;
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

  width: 50%;
  height: 4px;
  background: var(--color);
  box-shadow: 
    0 0 10px 0 var(--color1),
    0 0 20px 0 var(--color2),
    0 0 40px 0 var(--color3),
    0 0 80px 0 var(--color4);
  position: absolute;
}

.line:nth-of-type(1),
.line:nth-of-type(2) {
  top: 30%;
}

.line:nth-of-type(3),
.line:nth-of-type(4) {
  top: 48%;
}

.line:nth-of-type(5),
.line:nth-of-type(6) {
  top: 66%;
}

.line:nth-of-type(odd) {
  left: 0;
  width: 45%;
}

.line:nth-of-type(even) {
  right: 0;
}

.pointer {
  background: #000;
  transform: translate(-50%, -50%);
  width: 16px;
  height: 64px;
  border-radius: 9999px;
  position: absolute;
}

.controller {
  z-index: 2;
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 48px;
}
</style>
