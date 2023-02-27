<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input 
        type="text" 
        @input="getSearchResults"
        v-model="searchQuery"
        placeholder="Search for a city or state" 
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"/>
    </div>
    <ul
      v-if="mapboxSearchResults"
      class="relative bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[-1.5rem]">
      <li 
        :key="searchResult.id"
        v-for="searchResult in mapboxSearchResults"
        class="p-2 cursor-pointer ">
        {{ searchResult.place_name }}
      </li>
    </ul>
  </main>
</template>

<script setup>
  import { ref } from 'vue'
  import axios from 'axios'

  const mapboxAPIKey = 'pk.eyJ1Ijoiam95LW9hdC10ZWNoIiwiYSI6ImNsZWtnMTllZzBreHozb25tOWNoZHMxd2IifQ.4c81etTASs-qUqdss2dHSg'
  const searchQuery = ref('')
  const queryTimeout = ref(null)
  const mapboxSearchResults = ref(null)

  const getSearchResults = () => {
    clearTimeout(queryTimeout.value)
    queryTimeout.value = setTimeout(async () => {
      if (searchQuery.value !== '') {
        const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&types=place`)
        mapboxSearchResults.value = result.data.features
        return 
      }
      mapboxSearchResults.value = null
    }, 300)
  }
</script>


<style lang="scss" scoped>

</style>


