<template>
  <div class="dashboard">
    <div class="dashboard-content">
      <div>
        <h4>{{ username || "player" }}</h4>
        <h4>{{ userPick }}</h4>
        <p class="score">{{ userScore }}</p>
      </div>
      <div>
        <h4>computer</h4>
        <h4>{{ computerWeapon }}</h4>
        <p class="score">{{ computerScore }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: [
    "userPick",
    "computerWeapon",
    "userScore",
    "computerScore",
    "username",
  ],
  setup(props) {},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Keania+One&display=swap");

.dashboard {
  width: 50vw;
  margin: 4rem auto;
}

.dashboard-content {
  width: 100%;
  display: flex;
  justify-content: space-around;
  box-shadow: 1px 1px 2px black;
  padding: 0.5rem;
}

.dashboard-content h4 {
  width: 8rem;
  text-transform: uppercase;
  text-align: center;
  font-size: 1.2rem;
  padding: 0.3rem;
  color: rgb(51, 87, 87);
  font-family: "Josefin Sans", sans-serif;
}

.score {
  background: black;
  font-size: 3rem;
  color: rgb(32, 179, 166);
  padding: 5px;
  border-radius: 5px;
  font-family: "Keania One", cursive;
}

@media screen and (max-width: 768px) {
  .dashboard-content h4 {
    color: white;
    width: 45vw;
    font-size: 0.8rem;
    padding: 1rem 2rem 1rem 2rem;
    font-family: "Josefin Sans", sans-serif;
    border: none;
    background: rgb(62, 155, 119);
    border: 2px solid white;
    height: 8vh;
  }
  .dashboard-content {
    box-shadow: none;
  }

  .score {
    background: black;
    font-size: 1.5rem;
    border: 2px solid white;
    color: white;
    padding: 5px;
    border-radius: 0;
    font-family: "Keania One", cursive;
  }
}
</style>