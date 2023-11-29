<template>
    <div v-for="city in savedCities" :key="city.id">
        <CityCard :city="city" @click="goToCityView(city)"/>
    </div>
    <p v-if="savedCities.length === 0">
    No Locations added. To Start tracking a location, search in the field above.</p>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";
import CityCard from "./CityCard.vue";
import { useRouter } from "vue-router";

const API_KEY =import.meta.env.VITE_API_KEY;

const savedCities = ref([]);

const getCities = async () => {
    if(localStorage.getItem("savedCities")){
        savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
    }

    const requests = [];

    savedCities.value.forEach((city) => {
        requests.push(
            axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${city.coords.lat}&lon=${city.coords.lng}&appid=${API_KEY}&units=imperial`)
        );
    });

    const weatherData = await Promise.all(requests);

    await new Promise((res)=> setTimeout(res,1000));

    weatherData.forEach((value,index) => {
        savedCities.value[index].weather = value.data;
    });


}

await getCities();

const router = useRouter();
const goToCityView = (city) => {
    router.push({
        name:'cityView',
        params:{state:city.state, city:city.city},
        query:{id:city.id,lat:city.coords.lat, lng:city.coords.lng},
    })
}

</script>
