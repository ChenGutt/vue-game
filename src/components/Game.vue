<template>
  <div class="game-header">
    <Modal
      v-if="showModal"
      :showInputField="showInputField"
      labelText="please enter your name"
      @onNameEnter="setUsername"
    />
    <Modal
      v-if="showMessage"
      :showInputField="!showInputField"
      :labelText="message"
      @onNameEnter="setUsername"
      @onConfirm="confirmNewGame"
    />
    <Header v-if="!isGameStarted" />
  </div>

  <div v-if="!isGameStarted">
    <div class="weapons-title">
      <h4>{{ title }}</h4>
    </div>
    <div>
      <div v-if="!picked.length" class="weapons">
        <Weapons
          v-for="weapon in weapons"
          :weapon="weapon"
          :key="weapon"
          @onUserPick="setUserPick"
        />
      </div>
      <div v-if="picked.length" class="weapons">
        <Weapons
          v-for="weapon in picked"
          :weapon="weapon"
          :key="weapon"
          @onUserPick="setUserPick"
        />
      </div>
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
    :computerWeapon="computerWeapon"
    :userScore="userScore"
    :computerScore="computerScore"
    :username="userName"
  />
  <Battle
    v-if="isBattleOn"
    :userWeapon="userPick"
    :computerWeapon="computerWeapon"
    @onUpdateScore="setScore"
    @gameOver="isGameStarted == false && isBattleOn == false"
    @onPlayAgain="playAgain"
  />
</template>

<script>
import { ref } from "@vue/reactivity";
import { launchConfetti } from "../utilities/confetti";
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
    const picked = ref([]);
    const userPick = ref(null);
    const userName = ref("");
    const isBattleOn = ref(false);
    const isGameStarted = ref(false);
    const title = ref("choose your weapon");
    const showInputField = ref(true);
    const showMessage = ref(false); //modal upon winning or losing
    const message = ref("");

    //generate a random number between 0-2 to match one of the array elements and assigned as computer's weapon
    const computerWeapon = ref(
      weapons.value[Math.floor(Math.random() * weapons.value.length)]
    );

    const returnRandom = () => {
      return (computerWeapon.value =
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
      picked.value = weapons.value.filter((item) => item === weapon);
      title.value = `your weapon is ${weapon}`;
    };

    //starts the actual battle
    const setBattleOn = (gameOn) => {
      isBattleOn.value = gameOn;
    };

    //setting the scores battle
    const setScore = (uScore, cScore) => {
      userScore.value += uScore;
      computerScore.value += cScore;
      if (userScore.value === 3) {
        setTimeout(() => {
          showMessage.value = true;
          message.value = "you are the winner!";
        }, 700);
        launchConfetti();
      }
      if (computerScore.value === 3) {
        setTimeout(() => {
          showMessage.value = true;
          message.value = "you lost the game :-(";
        }, 700);
      }
    };

    //resetting the values when starting a new round
    const playAgain = () => {
      isGameStarted.value = false;
      isBattleOn.value = false;
      userPick.value = null;
      picked.value = [];
      title.value = "choose your weapon";
    };

    //set name received from the modal component to username
    const setUsername = (name) => {
      showModal.value = !showModal.value;
      userName.value = name;
    };

    const confirmNewGame = () => {
      showMessage.value = false;
      computerScore.value = null;
      userScore.value = null;
      playAgain();
    };

    return {
      weapons,
      userPick,
      startGame,
      isGameStarted,
      setUserPick,
      setBattleOn,
      isBattleOn,
      computerWeapon,
      userScore,
      computerScore,
      setScore,
      playAgain,
      showModal,
      userName,
      setUsername,
      picked,
      title,
      showInputField,
      message,
      showMessage,
      confirmNewGame,
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
