<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input v-model="searchQuery" @input="getSearchResult" type="text" placeholder="Search for a City or State"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-sm" />
      <ul v-if="mapboxSearchResults"
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
        <li v-for="search in mapboxSearchResults" :key="search.id" class="p-1 cursor-pointer">
          {{ search.place_name }}
        </li>
      </ul>
    </div>
  </main>
</template>
<script setup>
import { ref } from "@vue/reactivity";
import axios from "axios";

const MAPBOX_KEY = "pk.eyJ1Ijoic2hhaGVkdXp6YW1hbiIsImEiOiJjbDRtanAyZjkwMG0wM2JwaGVzaGZvZ3dkIn0.fbMl0Vxs6MqeS9lquIc61g";
const searchQuery = ref('');
const mapboxSearchResults = ref('');
const queryTimeout = ref(null);

const getSearchResult = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value == '') {
      mapboxSearchResults.value = null;
      return;
    }
    const result = await axios.get(
      `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${MAPBOX_KEY}&types=place`
    )
    mapboxSearchResults.value = result.data.features;
    console.log(result);
  }, 300);
}
</script>

