<template>
    <div class="flex py-6 px-3 bg-weather-secondary rounded-md shadow-md cursor-pointer">
        <div class="flex flex-col flex-1">
            <h2 class="text-3xl">{{ value.city }}</h2>
            <h3>{{ value.state }}</h3>
        </div>
        <div class="flex flex-col gap-2">
            <p class="text-3xl gap-2 self-end">
                {{ Math.round(value.weather.main.temp) }}&deg;
            </p>
            <div class="flex gap-2">
                <span class="text-xs">H: {{ Math.round(value.weather.main.temp_max) }}&deg;</span>
                <span class="text-xs">L: {{ Math.round(value.weather.main.temp_min) }}&deg;</span>
            </div>
        </div>
    </div>
</template>

<script setup>

defineProps({
    value: {
        type: Object,
        default: () => { }
    }
})

</script>
