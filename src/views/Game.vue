<template>
  <div class="game-header">
    <Modal v-if="showModal" @onNameEnter="setUsername" />
    <Header v-if="!isGameStarted" />
  </div>

  <div v-if="!isGameStarted">
    <div class="weapons-title">
      <h4>choose your weapon</h4>
    </div>
    <div class="weapons">
      <Weapons
        v-for="weapon in weapons"
        :weapon="weapon"
        :key="weapon"
        @onUserPick="setUserPick"
      />
    </div>
    <Button
      text="play now"
      class="start-button"
      @click="startGame"
      :disabled="!userPick"
    />
  </div>
  <div class="counter-wrapper">
    <Countdown v-if="isGameStarted" @onBattleStart="setBattleOn" />
  </div>
  <DashBoard
    v-if="userPick != null && isBattleOn"
    :userPick="userPick"
    :computerWeapon="computerPick"
    :userScore="userScore"
    :computerScore="computerScore"
    :username="userName"
  />
  <Battle
    v-if="isBattleOn"
    :userWeapon="userPick"
    :computerWeapon="computerPick"
    @onUpdateScore="setScore"
    @gameOver="isGameStarted == false && isBattleOn == false"
    @onPlayAgain="playAgain"
  />
</template>

<script>
import { ref } from "@vue/reactivity";
import Weapons from "./Weapons.vue";
import Button from "../reusables/Button.vue";
import DashBoard from "./DashBoard.vue";

import Battle from "./Battle.vue";
import Countdown from "./Countdown.vue";
import Modal from "./Modal.vue";
import { onMounted } from "@vue/runtime-core";
import Header from "../reusables/Header.vue";
export default {
  name: "Game",
  components: {
    Weapons,
    Button,
    DashBoard,
    Countdown,
    Battle,
    Modal,
    Header,
    Countdown,
  },
  setup() {
    onMounted(() => {
      showModal.value = true;
    });
    const showModal = ref(false);
    const weapons = ref(["paper", "rock", "scissors"]);
    const userScore = ref(null);
    const computerScore = ref(null);
    const userPick = ref(null);
    const userName = ref("");
    const computerPick = ref(
      weapons.value[Math.floor(Math.random() * weapons.value.length)]
    );
    const isGameStarted = ref(false);
    const isBattleOn = ref(false);

    const returnRandom = () => {
      return (computerPick.value =
        weapons.value[Math.floor(Math.random() * weapons.value.length)]);
    };

    //set start game status to true
    const startGame = () => {
      isGameStarted.value = true;
      returnRandom();
    };

    //set user's weapon
    const setUserPick = (weapon) => {
      userPick.value = weapon;
      console.log(userPick.value);
    };

    const setBattleOn = (gameOn) => {
      isBattleOn.value = gameOn;
    };
    const setScore = (uScore, cScore) => {
      userScore.value = uScore;
      computerScore.value = cScore;
    };

    const playAgain = () => {
      isGameStarted.value = false;
      isBattleOn.value = false;
      userPick.value = null;
    };

    const setUsername = (name) => {
      showModal.value = !showModal.value;
      userName.value = name;
    };

    return {
      weapons,
      userPick,
      startGame,
      isGameStarted,
      setUserPick,
      setBattleOn,
      isBattleOn,
      computerPick,
      userScore,
      computerScore,
      setScore,
      playAgain,
      showModal,
      userName,
      setUsername,
      returnRandom,
    };
  },
};
</script>
<style>
.game-header {
  color: white;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.weapons {
  display: flex;
  justify-content: center;
}

.weapons-title h4 {
  font-size: 2rem;
  font-family: "Josefin Sans", sans-serif;
}

@media screen and (max-width: 768px) {
  .weapons-title h4 {
    margin-bottom: 3rem;
  }
}
</style>
