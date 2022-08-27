<template>
    <div>
        <Suspense>
            <CityWeather />
            <template #fallback>
                <p class="text-white text-center pt-5">loading...</p>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import CityWeather from '../components/CityWeather.vue';

</script>

<style>
</style>