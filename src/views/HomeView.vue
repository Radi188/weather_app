<template>
  <main class="container">
    <div class="pt-4 mb-8 relative">
      <input type="text" v-model="searchQuery" @input="getSearchResult" placeholder="Search for a city or state" class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]">
      <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]" v-if="mapboxSearchResult">
        <p v-if="searchError">Sorry Something went wrong, Please try again</p>
        <p v-if="!serverError && mapboxSearchResult.length === 0">No result match your query, try different terms</p>
        <template v-else>
        
          <li  v-for="searchResult in mapboxSearchResult" :key="searchResult.id" class="py-2 cursor-pointer" @click="previewCity(searchResult)">
          {{ searchResult.place_name }}
        </li> 
          
        </template>
      </ul>
    </div>
    <div class="flex flex-col gap-4 ">
      <Suspense>
        <CityList />
        <template #fallback>
          <CityCardSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "@vue/reactivity";
import axios from "axios"
import { useRouter } from 'vue-router';
import CityList from "../components/CityList.vue";
import CityCardSkeleton from "../components/CityCardSkeleton.vue";

const router = useRouter();
const previewCity = (searchResult) => {
  const [city , state] = searchResult.place_name.split(",")
  router.push({
    name: "weather",
    params: {state: state.replaceAll(" ",""), city},
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true
    }
  })
}

 const searchQuery = ref("")
 const queryTimeout = ref(null)
 const mapboxSearchResult = ref(null)
 const mapboxApiKey = "pk.eyJ1IjoicmFkeTE4OCIsImEiOiJjbGN5NWt6aTcxbGxvM3Bwb2V3enUwZzVhIn0.7WL-IPnlZvpoCGJR3U7SIw"
 const searchError = ref(null)
 const getSearchResult = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async() => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(`https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxApiKey}&types=place`);
      mapboxSearchResult.value = result.data.features;
      console.log(mapboxSearchResult.value);

      }
      catch {
        searchError.value = true
      }
      return;
    }
    mapboxSearchResult.value = null;
  }, 300)

 };
</script>

<style lang="scss" scoped>

</style>