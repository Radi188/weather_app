<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="goToCityView(city)" />
    </div>
    <p v-if="savedCities.length === 0">No location added. to start tracking a location, search in the fields above</p>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import axios from "axios";
import { useRouter } from "vue-router";
import CityCard from "./CityCard.vue";

const savedCities = ref([]);
const getCities = async () => {
    if(localStorage.getItem('saveCities')) {
        savedCities.value = JSON.parse(localStorage.getItem('saveCities'));
    }
    const requests = [];
    savedCities.value.forEach((city) => {
        requests.push(
            axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=imperial`)
        )
    })
    const weatherData = await Promise.all(requests);

    await new Promise((res) => setTimeout(res, 1000))

    weatherData.forEach((value, index ) => {
        savedCities.value[index].weather = value.data;
    });

};
await getCities();
const router = useRouter();
const goToCityView = (city) => {
    router.push({
        name: 'weather',
        params: {state: city.state , city: city.city},
        query: { id: city.id,  lat: city.coords.lat , lng: city.coords.lng}
    })
}


</script>