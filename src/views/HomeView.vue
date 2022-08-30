<template>
    <main class="container text-white">
        <div class="pt-4 mb-8 relative">
            <input v-model="searchQuery" @input="getSearchResult" type="text" placeholder="Search for a City or State"
                class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-sm" />
            <ul v-if="mapboxSearchResults"
                class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]">
                <p v-if="searchError">Sorry something went wrong. Please try again.</p>
                <li v-if="mapboxSearchResults.length === 0 && !searchError">No result found. Try different one.</li>
                <template v-else>
                    <li v-for="searchResult in mapboxSearchResults" @click="previewCity(searchResult)"
                        :key="searchResult.id" class="p-1 cursor-pointer">
                        {{ searchResult.place_name }}
                    </li>
                </template>
            </ul>
        </div>
        <!-- Saved weaher -->
        <div class="flex flex-col gap-4" :class="{ 'hidden': mapboxSearchResults }">
            <CityList />
            <!-- <Suspense>
                <CityList />
                <template #fallback>
                    <p class="text-white text-center pt-5">Loading saved city...</p>
                </template>
            </Suspense> -->
        </div>
    </main>
</template>
<script setup>
import { ref } from "@vue/reactivity";
import axios from "axios";
import { useRouter } from "vue-router";
import CityList from "../components/CityList.vue";

const MAPBOX_KEY = "pk.eyJ1Ijoic2hhaGVkdXp6YW1hbiIsImEiOiJjbDRtanAyZjkwMG0wM2JwaGVzaGZvZ3dkIn0.fbMl0Vxs6MqeS9lquIc61g";
const searchQuery = ref('');
const mapboxSearchResults = ref('');
const queryTimeout = ref(null);
const searchError = ref(null);

const getSearchResult = () => {
    clearTimeout(queryTimeout.value);
    queryTimeout.value = setTimeout(async () => {
        if (searchQuery.value == '') {
            mapboxSearchResults.value = null;
            return;
        }
        try {
            const result = await axios.get(
                `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${MAPBOX_KEY}&types=place`
            )
            mapboxSearchResults.value = result.data.features;
        } catch {
            searchError.value = true;
            console.error(data);
        };

    }, 300);
}
const router = useRouter();
const previewCity = (result) => {
    const [city, state] = result.place_name.split(',');
    router.push({
        name: 'cityView',
        params: { city, state: state.replaceAll(" ", "") },
        query: {
            lat: result.geometry.coordinates[1],
            lng: result.geometry.coordinates[0],
            preview: true,
        }
    });
}
</script>

