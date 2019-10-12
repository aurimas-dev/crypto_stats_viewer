<template>
  <div id="app">
    <chart-select></chart-select>
    <g-chart
      v-if="chartData.length"
      :settings="{ packages: ['corechart', 'table', 'map'] }"
      type="CandlestickChart"
      :data="chartData"
      :options="chartOptions"
     />
    <!-- <crypto-list :markets="markets"></crypto-list> -->
  </div>
</template>

<script>
import CryptoList from './components/CryptoList.vue';
import ChartSelect from './components/ChartSelect.vue';
import {GChart} from 'vue-google-charts';
import {eventBus} from './main.js';


export default {
  name: 'app',
  components: {
    'g-chart': GChart,
    'crypto-list': CryptoList,
    'chart-select': ChartSelect,
  },
  data() {
    return {
      markets: [],
      chartData: [],
      chartOptions: {
        legend: 'none',
        bar: { groupWidth: '100%' }, // Remove space between bars.
        candlestick: {
          fallingColor: { strokeWidth: 0, fill: '#a52714' }, // red
          risingColor: { strokeWidth: 0, fill: '#0f9d58' }, // green
        },
        // TODO: another way to do horizontal scroll https://stackoverflow.com/questions/43788394/google-chart-timeline-horizontal-scroll?rq=1
        // explorer: {axis: 'horizontal'},
      },
      chartSelections: {},
    }
  },
  methods: {
    fetchChartData: function() {
      fetch(`https://poloniex.com/public?command=returnChartData&currencyPair=${this.chartSelections.currencyPair}&start=${this.chartSelections.start}&end=${this.chartSelections.end}&period=86400`)
      .then(response => response.json())
      .then(json => this.formatChartData(json));
    },
    formatChartData: function(data) {
      // Data should look like:
      // [['date', 'l', 'o', 'c', 'h'], [date, low, open, close, high], ...]
      const result = [];

      // Add headers
      result.push(['date', 'l', 'o', 'c', 'h']);

      // Add body
      for (let item of data) {
        result.push([item.date, item.low, item.open, item.close, item.high]);
      }

      this.chartData = result;
    }
  },
  mounted() {
    // Get the Markets data
    fetch('https://poloniex.com/public?command=returnCurrencies')
    .then(response => response.json())
    .then(json => this.markets = json);

    eventBus.$on('chart-select-submit', value => {
      this.chartSelections = value;
      this.fetchChartData();
    });
  },
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
