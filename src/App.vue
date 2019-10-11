<template>
  <div id="app">
    <crypto-list :markets="markets"></crypto-list>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import CryptoList from './components/CryptoList.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    'crypto-list': CryptoList
  },
  data() {
    return {
      markets: []
    }
  },
  mounted() {
    // Get the Markets data
    fetch('https://poloniex.com/public?command=returnCurrencies')
    .then(response => response.json())
    .then(json => this.markets = json);
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
