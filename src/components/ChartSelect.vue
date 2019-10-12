<template lang="html">
  <section>
    <span>Currency pairs:</span>
    <select v-model="chartSelections.currencyPair">
      <option value="" disabled selected>Select a currecy...</option>
      <option v-for="currencyPair in currencyPairs" :value="currencyPair">{{currencyPair}}</option>
    </select>

    <span>Start date:</span>
    <input type="date" v-model="chartSelections.start">

    <span>End date:</span>
    <input type="date" v-model="chartSelections.end">

    <!-- TODO: imcorporate date period -->

    <button type="button" v-on:click="handleSubmit">Show graph</button>
  </section>
</template>

<script>
import {eventBus} from '../main.js';

export default {
  name: 'chart-select',
  data() {
    return {
      currencyPairs: [],
      chartSelections: {
        start: new Date(new Date() - 86400000 * 90).toISOString().substr(0, 10),
        end: new Date().toISOString().substr(0, 10),
        currencyPair: 'USDT_BTC',
      },
    }
  },
  mounted() {
    fetch('https://poloniex.com/public?command=return24hVolume')
    .then(response => response.json())
    .then(json => this.currencyPairs = Object.keys(json).filter(value => !value.includes('total')));
  },
  methods: {
    handleSubmit: function() {
      // Input validation
      if (!this.chartSelections.currencyPair) {
        alert('Please select a currency pair.');
        return 0;
      }
      if (!this.chartSelections.start) {
        alert('Please select a start date.');
        return 0;
      }
      if (!this.chartSelections.end) {
        alert('Please select an end date.');
        return 0;
      }
      if (this.chartSelections.start > this.chartSelections.end) {
        alert('Start date can\'t be after the end date.');
        return 0;
      }
      // Pass on the data
      eventBus.$emit('chart-select-submit', {
        start: Math.floor(new Date(this.chartSelections.start) / 1000),
        end: Math.floor(new Date(this.chartSelections.end) / 1000),
        currencyPair: this.chartSelections.currencyPair,
      });
    },
  },
};
</script>

<style lang="css" scoped>
</style>
