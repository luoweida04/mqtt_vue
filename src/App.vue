<template>
  <div id="app">
    <indexVue></indexVue>
  </div>
</template>

<script>
import indexVue from './components/index.vue'

export default {
  name: 'App',
  components: {
    indexVue
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
