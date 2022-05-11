<template>
  <Line 
    :chart-data="chartData" :chart-options="chartOptions"
  />
</template>

<script>
import { Line } from "vue-chartjs";

export default{
  name: 'Chart',
  components: { Line },
  data () {
    return {
      chartData: null,
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    }
  },
  props:{
    label:{
      type: String
    },
    chartData:{
      type: Array
    }
  },
  mounted () {
    console.log(this.chartData)

    const dates = this.chartData.map(d => d.Date).reverse();
    const totals = this.chartData.map(d => d.Cases).reverse();

    this.chartData = {
      labels: dates,
      data: totals
    }
  }
}
</script>