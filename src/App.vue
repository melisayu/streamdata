<template>
 <div id="app">
  <h1 class="my-4">LIVE Stock Market Data</h1>
  <stockmarket></stockmarket>
 </div>
</template>

<script>
import Stockmarket from './components/Stockmarket'

export default {
  name: 'app',
  components: {
    Stockmarket
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
 display: flex;
 flex-direction: column;
 justify-content: center;
}

h1 {
  color: mediumturquoise;
}
</style>
