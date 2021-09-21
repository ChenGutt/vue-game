<template>
  <div
    :class="weaponClass"
    @click="weaponPickHandler(weapon)"
    :style="{
      backgroundImage:
        'url(' + require('@/assets/img/' + weapon + '.png') + ')',
    }"
  >
    {{ weapon }}
  </div>
</template>

<script>
import { ref } from "@vue/reactivity";

export default {
  props: ["weapon"],
  setup(props, { emit }) {
    const weaponClass = ref("weapon");
    const userWeapon = ref(null);
    const allowClick = ref(true);

    //choosing a weapon
    const weaponPickHandler = (weapon) => {
      if (allowClick.value) {
        userWeapon.value = weapon;
        
        emit("onUserPick", weapon);
      }
      allowClick.value = false;
    };
    return { weaponPickHandler, weaponClass, userWeapon };
  },
};
</script>

<style>
.weapon {
  margin: 1rem 1rem 0 0;
  cursor: pointer;
  width: 10rem;
  height: 10rem;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  display: grid;
  place-items: center;
  color: transparent;
}

.weapon.active {
  filter: brightness(40%);
}

.weapon:hover {
  color: white;
  border-radius: 50%;
  font-size: 1.7rem;
  font-weight: bold;
  font-family: "Josefin Sans", sans-serif;
  box-shadow: inset 100rem 100rem rgba(0, 0, 0, 0.637);
}

@media (max-width: 768px) {
  .weapon {
    margin: 0 auto;
    padding: 5px;
    width: 7rem;
    height: 7rem;
  }
}
</style>