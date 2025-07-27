<script setup lang="ts">
import { onMounted, useTemplateRef, watch } from "vue";

const props = defineProps<{ min: number; max: number }>();
const value = defineModel<number>({ required: true });
const ref = useTemplateRef("input");

onMounted(() => {
  setProgress(value.value);
});

watch(value, setProgress);

function setProgress(value?: number) {
  if (!value) return;
  const percent = ((value - props.min) / (props.max - props.min)) * 100;
  ref.value?.style.setProperty("--progress", `${percent}%`);
}
</script>

<template>
  <input
    ref="input"
    type="range"
    v-model.number="value"
    :min="props.min"
    :max="props.max"
  />
</template>
<style lang="css" scoped>
/*********** Baseline, reset styles ***********/
input[type="range"] {
  -webkit-appearance: none;
  appearance: none;
  background: transparent;
  cursor: pointer;
  width: 100%;

  --progress: 0%;
}

input[type="range"]::-webkit-slider-runnable-track {
  background: linear-gradient(
    to right,
    var(--color-green-200) var(--progress),
    var(--color-neutral-850) var(--progress)
  );
  border-radius: 0rem;
  height: 0.5rem;
}

input[type="range"]::-moz-range-track {
  background: linear-gradient(
    to right,
    var(--color-green-200) var(--progress),
    var(--color-neutral-850) var(--progress)
  );
  border-radius: 0rem;
  height: 0.5rem;
}

input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  margin-top: -10px;
  background-color: var(--color-neutral-200);
  border-radius: 100%;
  height: 1.75rem;
  width: 1.75rem;
}

input[type="range"]::-moz-range-thumb {
  margin-top: -10px;
  background-color: var(--color-neutral-200);
  border-radius: 100%;
  height: 1.75rem;
  width: 1.75rem;
}

input[type="range"]:active::-webkit-slider-thumb {
  border: 2px solid var(--color-green-300);
  background-color: var(--color-neutral-900);
}

input[type="range"]:active::-moz-range-thumb {
  border: 2px solid var(--color-green-300);
  background-color: var(--color-neutral-900);
}
</style>
