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
  </div>
</template>

<script>
import ChartSelect from './components/ChartSelect.vue';
import {GChart} from 'vue-google-charts';
import {eventBus} from './main.js';


export default {
  name: 'app',
  components: {
    'g-chart': GChart,
    'chart-select': ChartSelect,
  },
  data() {
    return {
      chartData: [],
      chartOptions: {
        legend: 'none',
        bar: { groupWidth: '100%' }, // Remove space between bars.
        candlestick: {
          fallingColor: { strokeWidth: 0, fill: '#a52714' }, // red
          risingColor: { strokeWidth: 0, fill: '#0f9d58' }, // green
        },
        hAxis: {
          slantedText: true,
          textStyle: {
            fontSize: 9,
          },
        },
        tooltip: {
          isHtml: true,
          textStyle: {
            fontSize: 12,
          },
        },
      },
      chartSelections: {},
    }
  },
  mounted() {
    eventBus.$on('chart-select-submit', value => {
      this.chartSelections = value;
      this.fetchChartData();
    });
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
        result.push([new Date(new Date(item.date * 1000)).toDateString().substring(4, 10), item.low, item.open, item.close, item.high]);
      }

      this.chartData = result;
    }
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
