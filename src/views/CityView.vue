<template>
    <div>
        <Suspense>
            <CityWeather />
            <template #fallback>
                <p>loading...</p>
            </template>
        </Suspense>
    </div>
</template>

<script setup>
import CityWeather from '../components/CityWeather.vue';

</script>

<style>
</style>