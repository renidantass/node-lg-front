<template>
  <ControllerGalaxyLG />
</template>

<script>
import ControllerGalaxyLG from "./components/ControllerGalaxyLG.vue";

export default {
  name: "App",
  components: {
    ControllerGalaxyLG,
  }
};
</script>

<style>
html,
body {
  background: #000;
  overflow-x: hidden;
  overflow-y: hidden;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
