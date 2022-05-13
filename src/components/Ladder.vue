<template>
  <div>
    <div class="flex flex-column">

      <!-- TOTAL CONFIRMED -->
      <div class="flex-1 mr-5 bg-blue-100 p-3 rounded-md shadow-lg">
        <div class="text-center mb-3">
          <span class="font-bold">Confirmed by Countries</span>
        </div>
        <div class="flex" v-for="(ladderDataConfirmedByCountry, index) in ladderDataConfirmedByCountries" :key="ladderDataConfirmedByCountry.Country">
          <div class="flex-auto">
            {{index+1}}. {{ ladderDataConfirmedByCountry.Country }} 
          </div>
          <div class="flex-2 text-left">
            {{ numberWithCommas(ladderDataConfirmedByCountry.TotalConfirmed) }}
          </div>
        </div>
      </div>

      <!-- TOTAL DEATHS -->
      <div class="flex-1 bg-red-400 p-3 rounded-md shadow-lg">
        <div class="text-center mb-3">
          <span class="font-bold">Deaths by Countries</span>
        </div>
        <div class="flex" v-for="(ladderDataDeathsByCountry, index) in ladderDataDeathsByCountries" :key="ladderDataDeathsByCountry.Country">
          <div class="flex-auto">
            {{index+1}}. {{ ladderDataDeathsByCountry.Country }} 
          </div>
          <div class="flex-2 text-left">
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
    }
  }
}
</script>