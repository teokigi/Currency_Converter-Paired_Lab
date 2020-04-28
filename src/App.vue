<template lang="html">
    <div id="app">
        <h1>Currency Converter</h1>
        <div class="userInput">
        <label for="Euros">User Currency Input: </label>
        <input type="number" name="Euros" placeholder="0" v-model="userCurrency" v-on:change="convertCurrency">
        </div>
        <div class="conversionRadio">
            Convert : 
            <label for="FromTo">From</label>
            <input type="radio" id="FromTo" name="FromTo" v-model="FromToConversionSelector" value="convertFrom" />
            <label for="FromTo">To</label>
            <input type="radio" id="FromTo" name="FromTo" v-model="FromToConversionSelector" value="convertTo"/>
        </div>

        <div class="ConversionSelector">
        <rates-list :rates="rates.rates"></rates-list>
        </div>
        <p>Calculated Conversion: {{this.convertCurrency() | toFixed}}</p>
    
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
        selectedCurrency: null,
        FromToConversionSelector: null
        }
    },
    filters:{
        toFixed: function(value){
            return value.toFixed(2)
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
                    if (this.FromToConversionSelector){
                        if (this.selectedCurrency){
                            if(this.FromToConversionSelector == "convertFrom"){
                                return (this.userCurrency * this.selectedCurrency);
                            } else if(this.FromToConversionSelector == "convertTo"){
                                return (this.userCurrency / this.selectedCurrency);
                            } else{
                                return "i can't do maths either"
                            }
                        }
                        else{
                            return "Please Select a Country Currency"
                        }
                    }
                    else {
                    return "Please Select conversion From or To"
                    }
                } else{
                return "Please Choose Input a number to Convert"
                }
            }
        }
    }
</script>

<style lang="css" scoped>
</style>
// Object.values(selectedCurrency)[0]
