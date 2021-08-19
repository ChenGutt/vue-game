<template>
  <div class="header-title"></div>
</template>

<script>
export default {
  props: ["title"],
};
</script>

<style>
.header-title {
  height: 40vh;
  background-image: url("../assets/img/coverImg.png");
  background-position: center;
  background-repeat: no-repeat;
  background-size: 50%;
}

@media (max-width: 768px) {
  .header-title {
    background-size: 100%;
  }
}
</style>