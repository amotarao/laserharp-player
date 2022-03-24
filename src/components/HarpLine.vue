<script setup>
import { defineProps, onBeforeUnmount, onMounted, ref, watch } from 'vue';

const props = defineProps({
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
</script>

<template>
  <div ref="line" :data-active="playing"></div>
</template>
