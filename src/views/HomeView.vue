<template>
  <main v-if="!loading">
    <DataTitleVue :text="title" :dataDate="dataDate" />

    <DataBoxesVue :stats="stats" />

    <CountrySelectVue @get-country="getCountryData" :countries="countries" />

    <button @click="clearCountryData" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
      Clear Country Selection
    </button>

  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
  </main>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import DataTitleVue from '@/components/DataTitle.vue';
import DataBoxesVue from '@/components/DataBoxes.vue';
import CountrySelectVue from '@/components/CountrySelect.vue';
export default defineComponent({
  name: 'HomeView',
  components: {
    DataTitleVue,
    DataBoxesVue,
    CountrySelectVue,
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
    }
  },
  methods: {
    async fetchCovidData() {
      const reqst = await fetch('https://api.covid19api.com/summary')
      const data = await reqst.json()
      return data
    },
    getCountryData(country: any) {
      this.stats = country
      this.title = country.Country
    },
    async clearCountryData() {
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = 'Global'
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
  },
});
</script>
