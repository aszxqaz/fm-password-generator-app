<script setup lang="ts">
const value = defineModel<boolean>({ required: true });
</script>

<template>
  <input type="checkbox" class="checkbox" v-model="value" />
</template>

<style scoped>
.checkbox {
  appearance: none;
  width: 1.25rem;
  height: 1.25rem;
  border: 0.125rem solid var(--color-neutral-200);

  &:hover,
  &:focus-visible {
    cursor: pointer;
    outline: none;
    border-color: var(--color-green-200);
  }

  &:checked {
    border-color: var(--color-green-200);
    background-color: var(--color-green-200);
    background-image: url("/images/icon-check.svg");
    background-repeat: no-repeat;
    background-position: center bottom 20%;
    position: relative;
  }
}
</style>
