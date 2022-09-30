<template>
  <main v-if="!loading">
    <DateTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />

    <button @click="refetchData()" v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-20 focus:outline-none hover:bg-green-600"> 
      clear Country
    </button>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-grey-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>

  </main>
</template>

<script>
const axios = require('axios')
import DateTitle from '@/components/DateTitle.vue'
import DataBoxes from '@/components/DataBoxes.vue'
import CountrySelect from '@/components/CountrySelect.vue'

export default {
  name: 'HomeView',
  components: {
    DateTitle,
    DataBoxes,
    CountrySelect,
    CountrySelect
},

  data () {
    return {
      loading:true,
      title: 'Global',
      dataDate: '',
      status: {},
      countries: [],
      loadingImage: ''
    }
  },
  methods: {
    async fetchCovidData(){
      const res = axios.get('https://api.covid19api.com/summary')
      const data = await res
      return data.data
    },
    getCountryData(country){
      this.stats = country
      this.title = country.Country
    },
    async refetchData(){
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = "Global"
      this.stats = data.Global
      this.loading = false
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false

  }
}
</script>
