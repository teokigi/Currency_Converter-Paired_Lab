<template lang="html">
  <div>
    <select v-model="selectedRate" v-on:select="appendSelectedCurrency">
      <option v-for="(rate, index) in rates" v-bind:value="rate">{{index}}: {{rate}}</option>
    </select>
  </div>
</template>

<script>
import {eventBus} from '@/main.js'

export default {
  name: 'rate-list',
  props: ['rates'],
  data() {
    return {
      selectedRate: null
    }
  },
  methods: {
    appendSelectedCurrency: function() {
      eventBus.$emit('selectHandle', this.selectedRate)
    }
  }
}
</script>

<style lang="css" scoped>
</style>
