<template>
  <div class="container">
    <div class="keyboard">
      <div class="wrapper plus" @click="slots--">-</div>
      <div class="wrapper" v-for="i in slots" :key="i" :data-spinned="0">
        <div class="door">
          <div
            class="boxes"
            @transitionstart="isTransitioning = true"
            @transitionend="isTransitioning = false"
            :style="{
              '--offset': `-${(pool.length - 1) * 200}px`,
            }"
          >
            <div
              class="box"
              v-for="letter in pool"
              :key="letter"
              :class="{ blur: isTransitioning }"
            >
              {{ letter }}
            </div>
          </div>
        </div>
        <div class="spinButton" @click="spin(i)">spin</div>
      </div>
      <div class="wrapper plus" @click="slots++">+</div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { ref, TransitionGroup } from "vue";
const alphabet = ref<string[]>("abcdefghijklmnopqrstuvwxyz".split(""));
const pool = ref<string[]>(["?"]);
const duration = ref(20);
const slots = ref(4);
const transitionIndex = ref(0);
const isTransitioning = ref(false);
const spin = async (index: number) => {
  pool.value.push(...shuffle(alphabet.value));
  pool.value.push(...shuffle(alphabet.value));
  setTimeout(() => {
    console.log("transition end");
    pool.value = [pool.value[pool.value.length - 1]];
  }, duration.value * 100);
};

const shuffle = (arr: string[]) => {
  let m = arr.length;
  while (m) {
    const i = Math.floor(Math.random() * m--);
    [arr[m], arr[i]] = [arr[i], arr[m]];
  }
  return arr;
};

const spinHandler = () => {};
</script>

<style lang="scss" scoped>
.blur {
  filter: blur(1px);
}
.container {
  .input {
    width: 500px;
    height: 40px;
    background-color: #252525;
    border-radius: 5px;
    margin-bottom: 10px;
  }
  .keyboard {
    align-items: center;
    display: flex;
    flex-wrap: wrap;
    background-color: rgb(26, 26, 26);
    border-radius: 8px;
    padding: 10px;
    gap: 10px;
    margin-bottom: 15px;
    position: relative;
    align-items: stretch;
    .plus {
      background: url("/src/assets/juice.webp");
      background-size: cover;
      background-position: 1000px 1000px;
      background-repeat: no-repeat;
      width: 100px;
      border-radius: 5px;
      background-color: #252525;
      justify-content: center;
      font-size: 6rem;
      cursor: pointer;
      &:hover {
        text-shadow: 0 0 1em black, 0 0 0.2em black;
        background-position: center center !important;
      }
    }
    .wrapper {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;

      .spinButton {
        background: url("/src/assets/gamba.webp");
        background-size: cover;
        background-position: 1000px 1000px;
        background-repeat: no-repeat;
        width: 100px;
        height: 50px;
        background-color: #252525;
        border-radius: 5px;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        font-size: 1.2rem;
        &:hover {
          text-shadow: 0 0 1em black, 0 0 0.2em black;
          background-position: center center !important;
        }
      }
    }
    .door {
      font-family: "Casino Flat Shadow", sans-serif;
      box-shadow: inset 0px 0px 10px rgba(0, 0, 0, 0.5);
      color: rgb(141, 10, 10);
      width: 100px;
      height: 200px;
      background-color: #a0a0a0;
      border-radius: 5px;
      display: flex;
      justify-content: center;
      overflow: hidden;
      .boxes {
        transition: transform 1s ease-in-out;
        transition-duration: 2s;
        transform: translateY(var(--offset));
        width: 100%;
        .box {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 100%;
          max-width: 100px;
          position: relative;
          height: 100%;
          font-size: 5rem;
        }
      }
    }
  }
}
</style>
