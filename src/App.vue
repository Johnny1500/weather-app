<template>
  <SearchCity @city-change="handleSearchCity" />
  <p>{{ cityInput }}</p>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import SearchCity from "./components/SearchCity.vue";
import { options, GEO_API_URL } from "./api";
import { debounce } from "lodash";

export default defineComponent({
  name: "App",
  components: {
    SearchCity,
  },
  data() {
    return {
      cityName: "",
      cityInput: "",
    };
  },
  watch: {
    cityInput(...args) {
      // eslint-disable-next-line
      // @ts-ignore
      this.searchFromGeoDB(...args);
    },
  },
  created() {
    // eslint-disable-next-line
    // @ts-ignore
    this.searchFromGeoDB = debounce(async (newCityInput, oldCityInput) => {
      try {
        const response = await fetch(
          `${GEO_API_URL}/cities?minPopulation=1000000&namePrefix=${newCityInput}`,
          options
        );
        const cities = await response.json();
        console.log("cities", cities);
      } catch (error) {
        console.log("error during fetch cities", error);
      }
    }, 1100);
  },
  beforeUnmount() {
    // eslint-disable-next-line
    // @ts-ignore
    this.searchFromGeoDB.cancel();
  },
  methods: {
    handleSearchCity(cityInput: string) {
      this.cityInput = cityInput;
    },
  },
});
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  max-width: 1080px;
  margin: 20px auto;
}

body {
  margin: 0;
  font-family: "Roboto", Arial !important;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #d5d4d4;
}
</style>
