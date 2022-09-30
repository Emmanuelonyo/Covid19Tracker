<template>
  <main v-if="!loading">
    <DateTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
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

export default {
  name: 'HomeView',
  components: { DateTitle, DataBoxes},
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
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    this.dataDate = data.Date
    this.stats = data.Global
    this.countries = data.countries
    this.loading = false

  }
}
</script>
