<script setup lang="ts">
import { reactive, ref } from "vue";
import Button from "./components/Button.vue";
import Checkbox from "./components/Checkbox.vue";
import Slider from "./components/Slider.vue";
import StrengthWidget from "./components/StrengthWidget.vue";

const state = reactive({
  charlen: 10,
  uppercase: true,
  lowercase: true,
  numbers: true,
  symbols: false,
});

const password = ref("");
const strength = ref<number>();
const copied = ref(false);

function calcStrength(setlen: number, passlen: number): number {
  const mul = setlen * passlen;
  console.log(mul);
  if (mul < 400) return 1;
  if (mul < 600) return 2;
  if (mul < 1000) return 3;
  return 4;
}

function generate() {
  const alphabet = {
    uppercase: "ABCDEFGHIJKLMNOPQRSTUVWXYZ",
    lowercase: "abcdefghijklmnopqrstuvwxyz",
    numbers: "01234567889",
    symbols: "?!@#$£%^&*()[]{}_-+=\\/'\"`~:;<>|.,",
  };

  const set = Object.keys(state)
    .filter((k) => k in alphabet && state[k as keyof typeof state])
    // @ts-ignore
    .reduce((acc, k) => [...acc, alphabet[k as keyof typeof alphabet]], [])
    // @ts-ignore
    .join("");

  password.value = new Array(state.charlen)
    .fill(0)
    .map((_) => set[Math.floor(Math.random() * set.length)])
    .join("");

  strength.value = calcStrength(set.length, password.value.length);
}

async function copy() {
  if (!password.value) return;
  await navigator.clipboard.writeText(password.value);
  copied.value = true;
  setTimeout(() => {
    copied.value = false;
  }, 1000);
}
</script>

<template>
  <div class="card">
    <h1 class="card__heading">Password Generator</h1>
    <div class="surface card__display display">
      <p :class="['display__password', { active: password }]">
        {{ password || "P4$5W0rD!" }}
      </p>
      <div class="display__copy-btn-group">
        <button class="display__copy-btn" @click="copy">
          <img src="/images/icon-copy.svg" />
        </button>
        <Transition name="fade">
          <span class="surface" v-if="copied">copied</span>
        </Transition>
      </div>
    </div>
    <div class="surface card__controls">
      <div class="card__charlen charlen">
        <p>Character Length</p>
        <p class="charlen__display">{{ state.charlen }}</p>
      </div>
      <Slider class="card__slider" v-model="state.charlen" :min="5" :max="20" />
      <label class="form-group">
        <Checkbox name="include-uppercase" v-model="state.uppercase" />
        Include Uppercase Letters
      </label>
      <label class="form-group">
        <Checkbox name="include-lowercase" v-model="state.lowercase" />
        Include Lowercase Letters
      </label>
      <label class="form-group">
        <Checkbox name="include-numbers" v-model="state.numbers" />
        Include Numbers
      </label>
      <label class="form-group">
        <Checkbox name="include-symbols" v-model="state.symbols" />
        Include Symbols
      </label>
      <StrengthWidget class="card__strength" :index="strength || 0" />
      <Button @click="generate">Generate</Button>
    </div>
  </div>
</template>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.card {
  width: 100%;
  max-width: 33.75rem;
}

.card__heading {
  font-size: clamp(1rem, 0.523rem + 2vw, 1.5rem);
  color: var(--color-neutral-600);
  margin-bottom: clamp(1rem, 0.046rem + 4vw, 2rem);
  text-align: center;
}

.card__display {
  margin-bottom: clamp(1rem, 0.523rem + 2vw, 1.5rem);
  /* padding-block: clamp(1rem, 0.82rem + 0.76vw, 1.1875rem); */
  padding-inline: clamp(1rem, 0.046rem + 4vw, 2rem);
}

.card__controls {
  padding: clamp(1rem, 0.523rem + 2vw, 1.5rem) clamp(1.5rem, 1rem + 2vw, 2rem);
}

.card__slider {
  margin-bottom: 2rem;
}

.card__strength {
  margin-bottom: 2rem;
}

.form-group {
  display: flex;
  gap: 1.5rem;
  user-select: none;
  cursor: pointer;

  &:not(:last-child) {
    margin-bottom: 1rem;
  }
}

.display {
  overflow: hidden;

  height: clamp(4rem, 3rem + 4vw, 5rem);
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.display__password {
  min-width: 0;
  text-overflow: ellipsis;
  overflow: hidden;
  color: var(--color-neutral-700);
  font-size: clamp(1.5rem, 1rem + 2vw, 2rem);
  margin-right: auto;
}

.display__password.active {
  color: var(--color-neutral-0);
}

.display__copy-btn-group {
  position: relative;

  display: flex;
  align-items: center;
  gap: 1rem;

  user-select: none;

  span {
    box-shadow: 0 0 1rem 1rem var(--color-neutral-800);
    z-index: 1;
    position: absolute;
    left: clamp(-0.5rem, -0.0223rem - 2.0356vw, -1rem);
    top: 50%;
    transform: translate(-100%, -50%);
    text-transform: uppercase;
    color: var(--color-green-200);
    font-size: clamp(1rem, 0.523rem + 2vw, 1.5rem);
  }
}

.display__copy-btn {
  position: relative;
  z-index: 2;
  background: none;
  border: none;
  cursor: pointer;

  &:hover {
    filter: brightness(0) invert(1);
  }
}

.charlen {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.charlen__display {
  font-size: clamp(1.5rem, 1rem + 2vw, 2rem);
  color: var(--color-green-200);
}
</style>
