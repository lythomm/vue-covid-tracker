<template>
  <main v-if="!loading">
    <div v-if="fetching" class="fetchingSpinner">
      <self-building-square-spinner
        class="spinner"
        :animation-duration="6000"
        :size="60"
        color="#ff1d5e"
      />
    </div>

    <DataTitle :text="title" :dataDate="dataDate" />
    
    <DataBoxes :stats="stats" />
    
    <div class="flex">
      <CountrySelect class="shadow-md" :countries="countries" @get-country="getCountryData" />
      <button @click="clearCountryData" v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10 ml-3 focus:outline-none hover:bg-green-600 shadow-md">
        <svg style="width:24px;height:24px" viewBox="0 0 24 24">
          <path fill="currentColor" d="M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2C6.47,2 2,6.47 2,12C2,17.53 6.47,22 12,22C17.53,22 22,17.53 22,12C22,6.47 17.53,2 12,2M14.59,8L12,10.59L9.41,8L8,9.41L10.59,12L8,14.59L9.41,16L12,13.41L14.59,16L16,14.59L13.41,12L16,9.41L14.59,8Z" />
        </svg>
      </button>
    </div>
      
    <Bar class="mt-10" v-if="stats.Country" :chart-data="chartData" :options="chartOptions" />

    <Ladder class="mt-10 mb-10" :covidData="countries" @getCountryFromChild="getCountryData" />

  </main>
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data...
    </div>
    <div v-if="fetching" class="fetchingSpinner">
      <self-building-square-spinner
        class="spinner"
        :animation-duration="6000"
        :size="60"
        color="#ff1d5e"
      />
    </div>
  </main>
</template>

<script>
import axios from "axios";
import DataTitle from "@/components/DataTitle"
import DataBoxes from "@/components/DataBoxes"
import CountrySelect from "@/components/CountrySelect"
import Ladder from "@/components/Ladder"
import moment from 'moment'
import { useToast } from "vue-toastification";
import { Bar, Line } from 'vue-chartjs'
import { SelfBuildingSquareSpinner  } from 'epic-spinners'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, LineElement, PointElement } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, LineElement, PointElement)
ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale,
  useToast
)
export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
    Line,
    Bar,
    SelfBuildingSquareSpinner,
    Ladder
  },
  data () {
    return {
      loading: true,
      fetching: false,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      deathsByDay: [],
      confirmedByDay: [],
      loadingImage: require('../assets/hourglass.gif'),
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      },
      chartData: {
        labels: [ 'January', 'February', 'March'],
        datasets: [
          {
            label: 'Data One',
            data: [40, 20, 12]
          }
        ]
      }
    }
  },
  async created() {
    const data = await this.fetchCovidData();

    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  },
  methods: {
    async fetchCovidData() {
      const res = await axios.get('https://api.covid19api.com/summary');
      return res.data;
    },
    async fetchCovidDataConfirmedByDay(country) {
      const res = await axios.get(`https://api.covid19api.com/dayone/country/${country}/status/confirmed`)
      return res.data
    },
    async fetchCovidDataDeathsByDay(country) {
      const res = await axios.get(`https://api.covid19api.com/dayone/country/${country}/status/deaths`)
      return res.data
    },
    async getCountryData (country) {
      console.log({country})
      this.fetching = true
      try {
        this.confirmedByDay = await this.fetchCovidDataConfirmedByDay(country.Country)
        this.deathsByDay = await this.fetchCovidDataDeathsByDay(country.Country)
      } catch (error) {
        this.sendNotif()
        console.error(error)
        this.fetching = false
        this.clearCountryData()
      }
      this.fetching = false
      this.formatData()
      this.stats = country
      this.title = country.Country
    },
    sendNotif () {
      const toast = useToast()

      toast.error("An error has occurred...", {
        timeout: 4000,
        pauseOnFocusLoss: false,
        closeOnClick: false,
        pauseOnHover: false,
        hideProgressBar: true,
      })
    },
    formatData () {
      const dates = this.confirmedByDay.map(d => moment(d.Date).format('MMM Do YYYY'))
      const totalsConfirmed = this.confirmedByDay.map(d => d.Cases)
      const totalDeaths = this.deathsByDay.map(d => d.Cases)

      this.chartData = null

      this.chartData = {
        labels: dates,
        datasets: [
          {
            label: 'Deaths',
            backgroundColor: 'red',
            data: totalDeaths
          },
          {
            label: 'Confirmed',
            backgroundColor: '#93c5fd',
            data: totalsConfirmed
          }
        ]
      }
    },
    async clearCountryData() {
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = 'Global'
      this.stats = data.Global
      this.loading = false
    }
  },
};
</script>

<style scoped>
.fetchingSpinner {
  position: fixed;
  top: 50%;
  left: 48%;
}
</style>