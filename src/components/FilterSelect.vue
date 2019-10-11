<template lang="html">
  <select v-model="filterSelected" v-on:change="handleSelect">
    <option v-for="filter in filters" :value="filter">{{filter}}</option>
  </select>
</template>

<script>
import {eventBus} from '../main.js'

export default {
  name: 'filter-selected',
  data() {
    return {
      filterSelected: '',
      filters: [
        'All',
        'Only available'
      ]
    }
  },
  methods: {
    handleSelect: function() {
      eventBus.$emit('filter-select', this.filterSelected);
    }
  }
}
</script>

<style lang="css" scoped>
</style>
