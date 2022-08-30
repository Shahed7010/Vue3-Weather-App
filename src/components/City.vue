<template>
    <div class="relative">
        <div @click="removeCity" class="absolute -top-2 -right-1 h-16  cursor-pointer" title="Remove from list">
            <i class="fa fa-times-circle opacity-20 text-2xl hover:opacity-100"></i>
        </div>
        <div @click="$emit('viewMore')" class="flex py-4 px-3 bg-weather-secondary rounded-md shadow-md cursor-pointer">
            <div class="flex flex-col flex-1">
                <h2 class="text-3xl">{{ value.city }}</h2>
                <h3>{{ value.state }}</h3>
            </div>
            <div class="flex flex-col gap-2">
                <p class="text-3xl gap-2 self-end">
                    {{ Math.round(value.weather.main.temp) }}&deg;
                </p>
                <div class="flex gap-2 justify-end">
                    <span class="text-md">H: {{ Math.round(value.weather.main.temp_max) }}&deg;</span>
                    <span class="text-md">L: {{ Math.round(value.weather.main.temp_min) }}&deg;</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const emit = defineEmits(['removed'])
const props = defineProps({
    value: {
        type: Object,
        default: () => { }
    }
})
const removeCity = () => {
    const savedCities = JSON.parse(localStorage.getItem('savedCities'));
    const filteredCity = savedCities.filter((city) => city.id !== props.value.id);

    localStorage.setItem('savedCities', JSON.stringify(filteredCity));

    emit('removed');
}
</script>
