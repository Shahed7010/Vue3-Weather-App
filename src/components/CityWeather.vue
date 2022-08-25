<template>
    <div>
        weather
    </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";

const route = new useRoute();
const API_ID = "90a9c5187866b2586b7e2917b34d4800";
const getWeatherData = async () => {
    try {
        const weatherData = await axios.get(
            `https://api.openweathermap.org/data/3.0/onecall?lat=${route.query.lat}&lon=${route.query.lng}&appid=${API_ID}&units=imperial`
        )
        // cal current date & time
        const localOffset = new Date().getTimezoneOffset() * 60000;
        const utc = weatherData.data.current.dt * 1000 + localOffset;
        weatherData.data.currentTime =
            utc + 1000 * weatherData.data.timezone_offset;
        // cal hourly weather offset
        weatherData.data.hourly.forEach((hour) => {
            const utc = hour.dt * 1000 + localOffset;
            hour.currentTime =
                utc + 1000 * weatherData.data.timezone_offset;
        });
        return weatherData.data;
    } catch (err) {
        console.log(err);
    }
}
const weatherData = await getWeatherData();
console.log(weatherData);
</script>
