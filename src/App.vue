<template lang="html">
    <div id="app">
      <h1>Currency Converter</h1>
      <input type="number" name="Euros" value="0" v-model="userCurrency" v-on:change="convertCurrency">
      <p>Because you can't do math, I did it for you: {{this.convertCurrency()}}</p>
      <rates-list :rates="rates.rates"></rates-list>
    </div>
</template>

<script>
import RatesList from './components/RatesList.vue';
import {eventBus} from '@/main.js'

export default {
  name: 'app',
  components: {
    'rates-list': RatesList
  },
  data() {
    return {
      rates: [],
      userCurrency: null,
      selectedCurrency: null
    }
  },
  mounted() {
    this.getRates()

    eventBus.$on('selectHandle', selCur => this.selectedCurrency = selCur )
    },
    methods:{
        getRates(){
            fetch("https://api.exchangeratesapi.io/latest")
            .then(res=>res.json())
            .then(resData=> this.rates=resData)
        },
        convertCurrency(){
            if (this.userCurrency) {
              return this.userCurrency * Object.values(this.selectedCurrency)[0]
            }
            else {
              return "Please add a value greater than 0."
            }
        }
    }
}
</script>

<style lang="css" scoped>
</style>
// Object.values(selectedCurrency)[0]
