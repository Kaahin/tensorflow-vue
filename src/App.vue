<template>
  <TensorFlow msg="TensorFlow ML For Linear Function" />
</template>

<script>
import TensorFlow from "./components/TensorFlow.vue";

export default {
  name: "App",
  components: {
    TensorFlow,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
