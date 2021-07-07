<template>
  <div id="app">
    <simon />
  </div>
</template>

<script>
import Simon from '@/components/Simon.vue';

export default {
  name: 'App',
  components: {
    Simon,
  },
};
</script>

<style lang="scss">
@import url('assets/null.css');

body {
  height: 100vh;
  overflow: hidden;
  background-color: rgb(255, 218, 218);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul {
  list-style: none;
}
</style>
