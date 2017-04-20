<template>
  <div id="app" class="container">
    <img src="./assets/logo.png">
    <crash></crash>
  </div>
</template>

<script>
import Crash from './components/Crash'

export default {
  name: 'app',
  components: {
    Crash
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
img {
  max-width: 100%;
}
</style>
