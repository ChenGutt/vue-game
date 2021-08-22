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
  <p class="game-explained" v-if="!userPick && round === 0">
    the first one to reach 3 points is the winner
  </p>
  <div v-if="!isGameStarted">
    <div class="weapons-title">
      <p v-if="userPick">round {{ round }}</p>
      <h4>{{ title }}</h4>
    </div>
    <div>
      <div class="weapons">
        <Weapons
          v-for="weapon in availableWeapons"
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
import { computed, ref } from "@vue/reactivity";
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
    const userPick = ref(null);
    const userName = ref("");
    const isBattleOn = ref(false); //when actual battle starts
    const isGameStarted = ref(false);
    const round = ref(0);
    const title = ref("choose your weapon");
    const showInputField = ref(true); //tells the modal not to display an input field in the modal
    const showMessage = ref(false); //modal upon winning or losing
    const message = ref(""); // displays in modal to tell user whether he won or lost the game

    const availableWeapons = computed(() => {
      return weapons.value.filter((item) =>
        userPick.value != null ? item === userPick.value : item
      );
    });

    //generate a random number between 0-2 to match one of the array elements and assigned as computer's weapon
    const computerWeapon = ref(
      weapons.value[Math.floor(Math.random() * weapons.value.length)]
    );

    //returns a random number that is assigned to computerWeapons
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
      round.value++;
      title.value = `your weapon is ${weapon} `;
    };

    //starts the actual battle
    const setBattleOn = (gameOn) => {
      isBattleOn.value = gameOn;
    };

    //setting the scores battle
    const setScore = (uScore, cScore) => {
      userScore.value += uScore;
      computerScore.value += cScore;
      //upon user winning 3 times
      if (userScore.value === 3) {
        round.value = 0;
        setTimeout(() => {
          showMessage.value = true;
          message.value = "you are the winner!";
        }, 700);
        launchConfetti();
      }
      //upon computer winning 3 times
      if (computerScore.value === 3) {
        round.value = 0;
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
      title.value = "choose your weapon";
    };

    //set name received from the modal component to username
    const setUsername = (name) => {
      showModal.value = !showModal.value;
      userName.value = name;
    };

    //resetting values after user confirmed a new game
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
      round,
      computerWeapon,
      userScore,
      computerScore,
      setScore,
      playAgain,
      showModal,
      userName,
      setUsername,
      title,
      showInputField,
      message,
      showMessage,
      confirmNewGame,
      availableWeapons,
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

.weapons-title {
  font-family: "Josefin Sans", sans-serif;
}

.weapons-title p {
  display: inline-block;
  padding: 0.6rem;
  border-radius: 5px;
  font-size: 1rem;
  background: rgb(64, 102, 114);
  color: white;
  text-transform: uppercase;
  margin-bottom: 1rem;
}

.weapons-title h4 {
  font-size: 2rem;
}

.game-explained {
  display: inline-block;
  padding: 0.3rem;
  background: rgb(10, 124, 109);
  color: white;
  border-radius: 5px;
  margin-bottom: 0.8rem;
  font-family: "Josefin Sans", sans-serif;
  animation: flickering 0.9s linear infinite;
}

@keyframes flickering {
  to {
    transform: scale(1.1);
  }
}

@media screen and (max-width: 768px) {
  .weapons-title h4 {
    margin-bottom: 3rem;
    font-size: 1.5rem;
  }
}
</style>
