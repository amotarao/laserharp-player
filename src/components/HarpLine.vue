<script setup>
import { computed, defineProps, onBeforeUnmount, onMounted, ref, watch } from 'vue';

const props = defineProps({
  mode: {
    type: String,
    required: true,
    validator: (value) => ['full', 'touch'].includes(value),
  },
  position: {
    type: String,
    required: true,
    validator: (value) => ['left', 'right'].includes(value),
  },
  audioSrc: {
    type: String,
    required: true,
  },
  touching: {
    type: Boolean,
    default: false,
  },
  pointerRect: {
    type: Object,
    required: true,
  },
});

const playing = ref(false);
const line = ref();
const lineRect = ref({ top: 0, right: 0, bottom: 0, left: 0 });

const checkOverlap = (pointerRect) => {
  return !(
    lineRect.value.top > pointerRect.bottom ||
    lineRect.value.right < pointerRect.left ||
    lineRect.value.bottom < pointerRect.top ||
    lineRect.value.left > pointerRect.right
  );
}

watch(() => [props.touching, props.pointerRect], ([touching, pointerRect]) => {
  if (touching) {
    playing.value = checkOverlap(pointerRect);
  } else {
    playing.value = false;
  }
});

const calcLineRect = () => {
  lineRect.value = line.value.getBoundingClientRect();
}

onMounted(() => {
  calcLineRect();
  window.addEventListener('resize', calcLineRect);
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', calcLineRect);
});

const audio = ref();

watch(playing, (playing) => {
  if (props.mode === 'touch') {
    if (playing) {
      audio.value.currentTime = 0;
      audio.value.play();
    } else {
      audio.value.pause();
    }
  }
  if (props.mode === 'full') {
    if (playing) {
      audio.value.currentTime = 0;
      audio.value.play();
    }
  }
});

const handleEnded = () => {
  if (props.mode === 'touch') {
    if (playing.value) {
      audio.value.currentTime = 0;
      audio.value.play();
    }
  }
}

const coverStyle = computed(() => {
const isRight = props.position === 'right';

  return {
    background: '#000',
    position: 'absolute',
    top: '-80px',
    right: isRight ? '0px' : undefined,
    bottom: '-80px',
    left: isRight ? undefined : '0px',
    width: isRight ? `calc(100vw - ${props.pointerRect.right}px)` : `${props.pointerRect.left}px`,
  };
});
</script>

<template>
  <div ref="line" :data-active="playing">
    <audio ref="audio" :src="audioSrc" @ended="handleEnded" />
    <div v-if="touching && playing" class="cover" :style="coverStyle"></div>
  </div>
</template>
