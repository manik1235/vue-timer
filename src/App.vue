<template>
  <div id="app">
    <TheTimer/>
  </div>
</template>

<script>
import TheTimer from './components/TheTimer.vue'

export default {
  name: 'App',
  components: {
    TheTimer
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
