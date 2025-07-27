<script setup lang="ts">
import { computed } from "vue";

const props = defineProps<{
  index: number;
}>();

const strength = computed(
  () => props.index && ["easy", "low", "medium", "hard"][props.index - 1]
);
</script>

<template>
  <div class="strength">
    <h2 class="strength__title">Strength</h2>
    <p v-if="strength" class="strength__value">{{ strength }}</p>
    <div class="strength__bar" :class="{ filled: index > 0 }"></div>
    <div class="strength__bar" :class="{ filled: index > 1 }"></div>
    <div class="strength__bar" :class="{ filled: index > 2 }"></div>
    <div class="strength__bar" :class="{ filled: index > 3 }"></div>
  </div>
</template>

<style lang="css" scoped>
.strength {
  user-select: none;
  padding-inline: clamp(1rem, 0.0458rem + 4.07vw, 2rem);
  height: clamp(3.5rem, 2.55rem + 4vw, 4.5rem);
  display: flex;
  align-items: center;
  text-transform: uppercase;
  background-color: var(--color-neutral-850);
}

.strength__title {
  color: var(--color-neutral-600);
  font-size: clamp(1rem, 0.88rem + 0.51vw, 1.125rem);
  margin-right: auto;
}

.strength__value {
  color: var(--color-neutral-200);
  font-size: clamp(1.125rem, 0.767rem + 1.5267vw, 1.5rem);
  margin-right: 1rem;
}

.strength__bar {
  width: 0.625rem;
  height: 1.75rem;
  border: 0.125rem solid var(--color-neutral-200);

  &:not(:last-child) {
    margin-right: 0.5rem;
  }

  &.filled {
    background-color: var(--color-yellow-300);
    border-color: var(--color-yellow-300);
  }
}
</style>
