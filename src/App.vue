<template>
  <div id="app">
    <g-chart
      :settings="{ packages: ['corechart', 'table', 'map'] }"
      type="CandlestickChart"
      :data="chartData"
      :options="chartOptions"
     />
    <crypto-list :markets="markets"></crypto-list>
  </div>
</template>

<script>
import CryptoList from './components/CryptoList.vue';
import { GChart } from 'vue-google-charts'

export default {
  name: 'app',
  components: {
    'g-chart': GChart,
    'crypto-list': CryptoList
  },
  data() {
    return {
      markets: [],
      chartData: [
        ['day', 'a', 'b', 'c', 'd'],
        ['Mon', 20, 28, 38, 45],
        ['Tue', 31, 38, 55, 66],
        ['Wed', 50, 55, 77, 80],
        ['Thu', 77, 77, 66, 50],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
        ['Fri', 68, 66, 22, 15],
      ],
      chartOptions: {
        legend: 'none',
        bar: { groupWidth: '100%' }, // Remove space between bars.
        candlestick: {
          fallingColor: { strokeWidth: 0, fill: '#a52714' }, // red
          risingColor: { strokeWidth: 0, fill: '#0f9d58' }, // green
        },
      }
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
