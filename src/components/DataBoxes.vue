<template>
  <div class="grid md:grid-cols-2 gap-4">

    <!-- CONFIRMED -->
    <div class="shadow-xl bg-white p-10 text-center rounded-md">
      <h3 class="text-3xl text-blue-900 font-bold mb-4">
        Cases
      </h3>
      <div class="text-2xl mb-4">
        <span class="font-bold">New: </span>
        <span id="counterNewC"> 0</span>
      </div>
      <div class="text-2xl mb-4">
        <span class="font-bold">Total: </span>
        <span id="counterTotalC"> 0</span>
      </div>
    </div>

    <!-- DEATHS -->
    <div class="shadow-xl bg-white p-10 text-center rounded-md">
      <h3 class="text-3xl text-red-900 font-bold mb-4">
        Deaths
      </h3>
      <div class="text-2xl mb-4">
        <span class="font-bold">New: </span>
        <span id="counterNewD"> 0</span>
      </div>
      <div class="text-2xl mb-4">
        <span class="font-bold">Total: </span>
        <span id="counterTotalD"> 0</span>
      </div>
    </div>
  </div>
</template>

<script>
import { CountUp } from 'countup.js'

export default {
  name: 'DataBoxes',
  components: {
    CountUp
  },
  props: ['stats'],
  data () {
    return {
      NewConfirmed: this.stats.NewConfirmed,
      TotalConfirmed: this.stats.TotalConfirmed,
      NewDeaths: this.stats.NewDeaths,
      TotalDeaths: this.stats.TotalDeaths
    }
  },
  mounted() {
    this.startCount()
  },
  watch: {
    stats: function(newStats) {
      this.NewConfirmed = newStats.NewConfirmed
      this.TotalConfirmed = newStats.TotalConfirmed
      this.NewDeaths = newStats.NewDeaths
      this.TotalDeaths = newStats.TotalDeaths

      this.startCount()
    }
  },
  methods: {
    startCount () {
      let countUpNewC = new CountUp('counterNewC', this.NewConfirmed);
      let countUpTotalC = new CountUp('counterTotalC', this.TotalConfirmed);
      let countUpNewD = new CountUp('counterNewD', this.NewDeaths);
      let countUpTotalD = new CountUp('counterTotalD', this.TotalDeaths);
      countUpNewC.start();
      countUpTotalC.start();
      countUpNewD.start();
      countUpTotalD.start();
    }
  }
}
</script>

<style scoped>
  .iCountUp {
    font-size: 12em;
    margin: 0;
    color: #4d63bc;
  }
</style>