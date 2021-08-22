<template>
  <div :class="counter === 1 ? 'counterIsOne' : 'counter'" v-if="!gameOn">
    <span>{{ counter }}</span>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";

export default {
  setup(props, { emit }) {
    const counter = ref(3);
    let timer = ref(null);
    let stopTimer = ref(null);
    const gameOn = ref(false);

    onMounted(() => {
      timer = setInterval(() => {
        counter.value--;
        if (counter.value < 1) {
          stopTimer = clearInterval(timer);
          gameOn.value = true;
          emit("onBattleStart", gameOn.value);
        }
      }, 700);
    });

    return { counter, timer, stopTimer, gameOn };
  },
};
</script>

<style>
.counter,
.counterIsOne {
  border: 10px solid #2c3e50;
  border-radius: 50%;
  height: 25rem;
  width: 25rem;
  margin: 5rem auto;
  display: grid;
  place-items: center;
}

.counter span,
.counterIsOne span {
  font-size: 15rem;
}

.counterIsOne {
  animation: fading 1s forwards;
}

@keyframes fading {
  to {
    opacity: 0;
  }
}

@media (max-width: 768px) {
  .counter,
  .counterIsOne {
    height: 20rem;
    width: 20rem;
    margin: 10rem auto;
  }
}
</style>