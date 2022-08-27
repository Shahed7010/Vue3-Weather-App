<template>
    <div v-for="city in savedCities" :key="city.id">
        <City :value="city" @click="goWeatherView(city)" />
    </div>
    <h3 v-if="!savedCities.length" class="pt-5 text-center">There is no saved city. Search for a city to get weather
        data.</h3>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import axios from "axios";
import { useRouter } from "vue-router";
import City from "./City.vue";

const savedCities = ref([]);
const getCities = async () => {
    if (localStorage.getItem('savedCities')) {
        savedCities.value = JSON.parse(
            localStorage.getItem('savedCities')
        );
    }

    const API_ID = "90a9c5187866b2586b7e2917b34d4800";
    const requests = [];
    savedCities.value.forEach((city) => {
        console.log('req aice');
        requests.push(
            axios.get(
                `https://api.openweathermap.org/data/2.5/weather?lat=${city.cords.lat}&lon=${city.cords.lng}&appid=${API_ID}&units=metric`
            )
        );
    });
    const weatherData = await Promise.all(requests);

    weatherData.forEach((weather, index) => {
        console.log('data');
        savedCities.value[index].weather = weather.data;
    });
}

await getCities();

const router = useRouter();
const goWeatherView = (value) => {
    router.push({
        name: 'cityView',
        params: { city: value.city, state: value.state },
        query: { lat: value.cords.lat, lng: value.cords.lng }
    })
}
</script>
