<template>
  <div id="app">
    <Timer />
  </div>
</template>

<script>
import Timer from "./components/Timer.vue";
export default {
  name: "app",
  components: {
    Timer
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fafafa;
  /* margin-top: 60px; */
  height: 100vh;
  width: 100%;
  background: rgb(2, 0, 36);
  background: linear-gradient(
    0deg,
    rgba(2, 0, 36, 1) 0%,
    rgba(9, 9, 121, 1) 0%,
    rgba(0, 212, 255, 1) 100%
  );
}
</style>
