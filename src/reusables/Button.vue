<template>
  <button :class="class">{{ text }}</button>
</template>

<script>
export default {
  props: ["text", "class"],
  // setup(){

  //     return{  }
  // }
};
</script>

<style>
.start-button {
  margin-top: 3rem;
  background: black;
  color: white;
  padding: 0.9rem;
  border: 0;
  outline: none;
  border-radius: 40px;
  font-size: 0.8rem;
  text-transform: uppercase;
  cursor: pointer;
}

.start-button:disabled {
  background: rgba(0, 0, 0, 0.61);
  cursor: not-allowed;
}
</style>