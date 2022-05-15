<template>
  <div>
    <div class="flex flex-column">

      <!-- TOTAL CONFIRMED -->
      <div class="flex-1 mr-5 bg-white p-3 rounded-md shadow-lg">
        <div class="text-center text-blue-500 mb-3">
          <span class="font-bold">Confirmed by Countries</span>
        </div>
        <div class="flex" v-for="(ladderDataConfirmedByCountry, index) in ladderDataConfirmedByCountries" :key="ladderDataConfirmedByCountry.Country">
          <div class="flex-auto border-r-2 border-b-2 p-1">
            <span>{{index+1}}. </span> 
            <span class="cursor-pointer" @click="getCountry(ladderDataConfirmedByCountry)">{{ ladderDataConfirmedByCountry.Country }}</span>
          </div>
          <div class="flex-2 text-left border-b-2 p-1">
            {{ numberWithCommas(ladderDataConfirmedByCountry.TotalConfirmed) }}
          </div>
        </div>
      </div>

      <!-- TOTAL DEATHS -->
      <div class="flex-1 bg-white p-3 rounded-md shadow-lg">
        <div class="text-center text-red-600 mb-3">
          <span class="font-bold">Deaths by Countries</span>
        </div>
        <div class="flex" v-for="(ladderDataDeathsByCountry, index) in ladderDataDeathsByCountries" :key="ladderDataDeathsByCountry.Country">
          <div class="flex-auto border-r-2 border-b-2 p-1">
            <span>{{index+1}}. </span> 
            <span class="cursor-pointer" @click="getCountry(ladderDataDeathsByCountry)">{{ ladderDataDeathsByCountry.Country }}</span>
          </div>
          <div class="flex-2 text-left border-b-2 p-1">
            {{ numberWithCommas(ladderDataDeathsByCountry.TotalDeaths) }}
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import { CountUp } from 'countup.js'

export default {
  name: 'Ladder',
  components: {
    CountUp
  },
  props: ['covidData',],
  data () {
    return {
      ladderDataDeathsByCountries: [],
      ladderDataConfirmedByCountries: [],
    }
  },
  created () {
    this.sortData()
  },
  methods: {
    sortData() {
      let dataToSort = this.covidData.sort((a,b) => a.TotalDeaths - b.TotalDeaths)
      this.ladderDataDeathsByCountries = dataToSort.slice(Math.max(dataToSort.length - 10, 1)).reverse()
      let confirmedToSort = this.covidData.sort((a,b) => a.TotalConfirmed - b.TotalConfirmed)
      this.ladderDataConfirmedByCountries = confirmedToSort.slice(Math.max(confirmedToSort.length - 10, 1)).reverse()
    },
    startCount () {
      let countUpNewC = new CountUp('counterNewC', this.NewConfirmed);
      let countUpTotalC = new CountUp('counterTotalC', this.TotalConfirmed);
      let countUpNewD = new CountUp('counterNewD', this.NewDeaths);
      let countUpTotalD = new CountUp('counterTotalD', this.TotalDeaths);
      countUpNewC.start();
      countUpTotalC.start();
      countUpNewD.start();
      countUpTotalD.start();
    },
    numberWithCommas (x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
    },
    getCountry (country) {
      console.log(country)
      this.$emit('getCountryFromChild', country)
    }
  }
}
</script>