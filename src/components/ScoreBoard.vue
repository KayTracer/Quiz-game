<template>
  <section class="score">
    Player <span>{{ this.winCount }}</span> x
    <span>{{ this.loseCount }}</span> Computer
  </section>
</template>

<script>
export default {
  name: "ScoreBoard",
  props: ["winCount", "loseCount"],
};
</script>

<style lang="scss">
.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 20px;
}
span {
  font-weight: bold;
  border: 1px solid black;
  padding: 8px;
}
</style>
