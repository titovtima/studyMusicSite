<template>
  <canvas ref="canvas"/>
</template>

<script setup lang="ts">
const canvas: Ref<HTMLCanvasElement | null> = ref(null);

const props = defineProps({
  width: {
    type: Number,
    default: 1000
  },
  height: {
    type: Number,
    default: 200
  },
  pressed: {
    type: Array,
    default: []
  },
  octaves: {
    type: Number,
    default: 2
  }
});

onMounted(() => {
  if (canvas.value) {
    canvas.value.width = Math.min(props.width, props.height * 2.5 * props.octaves);
    canvas.value.height = props.height;
    drawPiano(canvas.value);
  }
});
watch(() => props.pressed, () => {
  if (canvas.value)
    drawPiano(canvas.value);
});

function whiteKeyPositionToId(position: number): number {
  let result = position * 2;
  for (let i = 0; i < props.octaves; i++) {
    if (position > 2 + i * 7) result--;
    if (position > 6 + i * 7) result--;
  }
  return result;
}

function blackKeyPositionToId(position: number): number {
  if (position % 7 == 2 || position % 7 == 6) return -1;
  let result = position * 2 + 1;
  for (let i = 0; i < props.octaves; i++) {
    if (position > 2 + i * 7) result--;
    if (position > 6 + i * 7) result--;
  }
  return result;
}

function drawPiano(canvas: HTMLCanvasElement) {
  let ctx = canvas.getContext("2d");
  if (!ctx)
    return;
  ctx.fillStyle = "#FFFFFF";
  ctx.strokeStyle = "#000000";
  ctx.lineWidth = 2;
  let width = canvas.width / (7 * props.octaves);
  for (let i = 0; i < 7 * props.octaves; i++) {
    ctx.fillRect(i * width, 0, width, canvas.height);
    ctx.strokeRect(i * width, 0, width, canvas.height);
  }
  ctx.fillStyle = "#000000";
  let height = canvas.height * 3 / 5;
  let width2 = width * 2 / 3;
  let margin = width * 2 / 3;
  for (let i = 0; i < 7 * props.octaves - 1; i++) {
    if (i % 7 == 2 || i % 7 == 6)
      continue;
    ctx.fillRect(i * width + margin, 0, width2, height);
  }
  ctx.fillStyle = "#FF7777";
  let padding = Math.min(5, width2 / 8);
  for (let i = 0; i < 7 * props.octaves; i++) {
    if (props.pressed.includes(whiteKeyPositionToId(i))) {
      ctx.fillRect(i * width + padding, height + padding,
        width - padding * 2, canvas.height - height - padding * 2);
    }
    if (props.pressed.includes(blackKeyPositionToId(i))) {
      ctx.fillRect(i * width + margin + padding, padding,
        width2 - padding * 2, height - padding * 2);
    }
  }
}
</script>

<style scoped>
canvas {
  max-width: 100vw;
}
</style>