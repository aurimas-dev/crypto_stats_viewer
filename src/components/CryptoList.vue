<template lang="html">
  <div>
    <filter-select></filter-select>
    <div class="table-responsive" v-if="filterMarkets">
      <table>
        <tr>
          <th>Initials</th>
          <th>Name</th>
          <th>Disabled</th>
        </tr>
        <tr v-for="(market, key) in filterMarkets">
          <td>{{key}}</td>
          <td>{{market.name}}</td>
          <td>{{market.disabled ? "Yes" : "No"}}</td>
        </tr>
      </table>
    </div>
    <div v-if="!filterMarkets">
      <p>No data available</p>
    </div>
  </div>
</template>

<script>
import FilterSelect from './FilterSelect.vue'
import {eventBus} from '../main.js'

export default {
  name: 'crypto-list',
  data() {
    return {
      filterSelected: "All"
    }
  },
  mounted() {
    eventBus.$on('filter-select', value => this.filterSelected = value);
  },
  computed: {
    filterMarkets: function() {
      let result = this.markets;

      switch(this.filterSelected) {
        case 'Only available':
          result = {};
          for (let key in this.markets) {
            if (!this.markets[key].disabled) {
              result[key] = this.markets[key];
            }
          }
          return result;
      }

      return result;
    }
  },
  components: {
    'filter-select': FilterSelect
  },
  props: ['markets']
}
</script>

<style lang="css" scoped>
.table-responsive {
  overflow-x: auto;
  margin: 0 auto;
}
</style>
