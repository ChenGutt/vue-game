<template>
  <div class="dashboard">
    <div class="dashboard-content">
      <div>
        <p>
          <strong>{{ username || "player" }}</strong>
        </p>
        <p>{{ userPick }}</p>
        <p>{{ userScore }}</p>
      </div>
      <div>
        <p>
          <strong>computer</strong>
        </p>
        <p>{{ computerWeapon }}</p>
        <p>{{ computerScore }}</p>
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
  setup(props) {
    console.log(props);
  },
};
</script>

<style>
.dashboard {
  width: 50vw;
  /* padding: 1rem; */
  margin: 4rem auto;
}

.dashboard-content {
  width: 100%;
  display: flex;
  justify-content: space-around;
}

@media screen and (max-width: 768px) {
  .dashboard-content p {
    color: white;
    width: 45vw;
    font-size: 1rem;
    padding: 1rem 2rem 1rem 2rem;
    font-family: "Josefin Sans", sans-serif;
  }

  .dashboard-content p {
    background: rgb(62, 155, 119);
  }

  .dashboard-content p strong {
    font-size: 1.3rem;
    text-transform: capitalize;
  }
}
</style>