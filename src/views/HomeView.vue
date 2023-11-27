<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        v-model="searchQuery"
        @input="getSearchResults"
        type="text"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-wather-secoundary focus:outline-none focus:shadpw-[0px_1px_0_0_#004E71]"
      />
      <ul
        class="absolute bg-wather-secoundary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="mapboxSearchResults"
        >
        <li
          v-for="searchResult in mapboxSearchResults"
          :key="searchResult.id"
          class="py-2 cursor-pointer"
        >
      {{ searchResult.place_name }}
      </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const mapBoxAPIKey =
  "pk.eyJ1IjoicmFuZGlrYTkzIiwiYSI6ImNscGdnNjZzdDFseXoyaW5zbGQ5bWpjYXgifQ.Nl2wgJOtF9TsjjdqcsiLoQ";

const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const results = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapBoxAPIKey}&types=place`
      );
      mapboxSearchResults.value = results.data.features;
      console.log(mapboxSearchResults.value);
      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>
