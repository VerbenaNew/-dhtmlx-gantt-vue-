<template>
  <div id="app">
    <GanttView />
  </div>
</template>

<script>
import GanttView from "./components/GanttView.vue";

export default {
  name: "App",
  components: {
    GanttView,
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
