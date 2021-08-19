<template>
  <div class="backdrop">
    <div class="modal">
      <label>please enter your name</label>
      <input type="text" v-model="username" /><br />
      <Button text="enter" class="start-button" @click="enterName" />
    </div>
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";
import Button from "../reusables/Button.vue";
export default {
  components: { Button },
  emits: ["onUserInput", "onNameEnter"],
  setup(props, { emit }) {
    const username = ref("");

    emit("onUserInput", username.value);

    const enterName = () => {
      emit("onNameEnter", username.value);
      console.log(username.value);
    };

    return { username, enterName };
  },
};
</script>

<style>
.backdrop {
  position: fixed;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.678);
  display: grid;
  place-items: center;
  inset: 0;
  z-index: 10;
}

.modal {
  position: relative;
  width: 30vw;
  min-width: 200px;
  height: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  text-transform: uppercase;
  font-family: "Josefin Sans", sans-serif;
  border-radius: 5px;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.384);
  padding: 1rem;
  color: black;
  background: white;
  z-index: 100;
}

.modal input {
  border-radius: 5px;
  padding: 0.5rem;
  margin-top: 1rem;
  outline: none;
}
</style>