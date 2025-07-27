<template>
  <button class="button">
    <slot></slot
    ><svg width="12" height="12" xmlns="http://www.w3.org/2000/svg">
      <path
        fill="currentColor"
        d="m5.106 12 6-6-6-6-1.265 1.265 3.841 3.84H.001v1.79h7.681l-3.841 3.84z"
      />
    </svg>
  </button>
</template>

<style lang="css" scoped>
.button {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;

  padding-block: 0.9em;

  border: 2px solid var(--color-green-200);
  color: var(--color-neutral-800);
  background-color: var(--color-green-200);
  font: inherit;
  text-transform: uppercase;

  cursor: pointer;

  &:active {
    background-color: var(--color-neutral-800);
    color: var(--color-green-200);
  }
}
</style>
