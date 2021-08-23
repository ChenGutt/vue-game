<template>
  <div class="battle">
    <div
      class="user"
      :style="{
        backgroundImage:
          'url(' + require('@/assets/img/' + userWeapon + '.png') + ')',
      }"
    ></div>

    <div
      class="computer"
      :style="{
        backgroundImage:
          'url(' + require('@/assets/img/' + computerWeapon + '.png') + ')',
      }"
    ></div>
  </div>
  <div :class="winner">{{ result }}</div>
  <Button text="play again" class="button" @click="playAgain" />
</template>

<script>
import { ref } from "@vue/reactivity";
import Button from "../reusables/Button.vue";
export default {
  components: { Button },
  emits: ["onUpdateScore", "onPlayAgain", "gameOver"],
  props: ["userWeapon", "computerWeapon"],
  setup({ userWeapon, computerWeapon }, { emit }) {
    const result = ref(null);
    const winner = ref("winner");
    const userScore = ref(0);
    const computerScore = ref(0);

    //emits an event to parent component to update score
    const updatedScore = () => {
      emit("onUpdateScore", userScore.value, computerScore.value);
    };

    //upon draw
    const draw = () => {
      result.value = "It's a draw";
      winner.value = "winner draw";
    };

    //upon winning
    const win = () => {
      result.value = "you win!";
      userScore.value++;
      winner.value = "winner win";
    };

    //upon losing
    const lose = () => {
      result.value = "you lose";
      computerScore.value++;
      winner.value = "winner lose";
    };

    //emitting an event to start a new game
    const playAgain = () => {
      emit("onPlayAgain");
    };

    const checkWinner = () => {
      //check if draw
      if (userWeapon === computerWeapon) {
        draw();
      } else if (userWeapon === "scissors") {
        //if user has scissors
        if (computerWeapon === "paper") {
          win();
        } else if (computerWeapon === "rock") {
          lose();
        }
        //if user has rock
      } else if (userWeapon === "rock") {
        if (computerWeapon === "scissors") {
          win();
        } else if (computerWeapon === "paper") {
          lose();
        }

        //if user has paper
      } else if (userWeapon === "paper") {
        if (computerWeapon === "rock") {
          win();
        } else if (computerWeapon === "scissors") {
          lose();
        }
      }
      emit("gameOver");
      updatedScore();
    };
    checkWinner();

    return { result, checkWinner, winner, updatedScore, playAgain };
  },
};
</script>

<style>
.battle {
  display: flex;
  justify-content: center;
}

.computer,
.user {
  width: 10rem;
  height: 10rem;
  margin: 0 1.5rem;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  font-size: 3rem;
}

.winner {
  text-transform: uppercase;
  width: 20%;
  margin: 3rem auto 0;
  color: white;
  padding: 0.5rem;
  border-radius: 5px;
  font-weight: bold;
}

.win {
  background: rgb(0, 128, 85);
}

.lose {
  background: rgb(221, 57, 66);
}

.draw {
  background: rgb(148, 130, 148);
}

@media screen and (max-width: 768px) {
  .computer,
  .user {
    width: 7rem;
    height: 7rem;
  }

  .winner {
    width: 40%;
  }
}
</style>