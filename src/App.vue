<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <TicTacToe />
  </div>
</template>

<script>
import TicTacToe from './components/TicTacToe.vue'

export default {
  name: 'app',
  components: {
    TicTacToe
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
</style>
