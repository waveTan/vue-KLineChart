<template>
  <div id="app">
    <img alt="Vue logo" class="logo" src="./assets/logo.png">
    <KlinChart>
    </KlinChart>
  </div>
</template>

<script>
  import KlinChart from './components/KlinChart.vue'

  export default {
    name: 'App',
    components: {
      KlinChart
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    background-color: #0b0c14;
  }

  .logo {
    width: 100px;
  }
</style>
