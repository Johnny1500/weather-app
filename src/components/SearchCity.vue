<template>
  <div>
    <input
      type="text"
      placeholder="Search for city"
      v-model="cityInput"
      id="search-city-input"
    />
    <div id="selected-cities-array" v-show="selectedGeoDBCities.length > 0">
      <ul v-if="!loadingCities">
        <li
          v-for="item in selectedGeoDBCities"
          :key="item['id']"
          class="selected-city"
        >
          {{ item["name"] }}
        </li>
      </ul>
      <div v-else>Loading...</div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { geoOptions, GEO_API_URL } from "../api";

export default defineComponent({
  name: "SearchCity",
  // emits: ["cityChange"],
  // data() {
  //   return {
  //     selectedGeoDBCities: [],
  //     cityInput: "",
  //     loadingCities: false,
  //     isCityInputFocus: true,
  //   };
  // },
  // watch: {
  //   cityInput(...args) {
  //     // eslint-disable-next-line
  //     // @ts-ignore
  //     this.searchFromGeoDB(...args);
  //   },
  // },
  // created() {
  //   // eslint-disable-next-line
  //   // @ts-ignore
  //   this.searchFromGeoDB = debounce(async (newCityInput) => {
  //     try {
  //       this.loadingCities = true;

  //       const response = await fetch(
  //         `${GEO_API_URL}/cities?minPopulation=1000000&namePrefix=${newCityInput}`,
  //         geoOptions
  //       );
  //       const cities = await response.json();
  //       // console.log("cities", cities);

  //       this.selectedGeoDBCities = cities.data;

  //       this.loadingCities = false;
  //     } catch (error) {
  //       console.log("error during fetch cities", error);
  //     }
  //   }, 1200);
  // },
  // beforeUpdate() {
  //   console.log(this);
  // },
  // beforeUnmount() {
  //   // eslint-disable-next-line
  //   // @ts-ignore
  //   this.searchFromGeoDB.cancel();
  // },
  // methods: {
  //   handleSelectedSearchCity(city: string) {
  //     this.cityInput = city;
  //     this.$emit("cityChange", city);
  //     this.isCityInputFocus = false;
  //   },
  //   handleCityInputFocus() {
  //     this.isCityInputFocus = true;
  //   },
  // },
});
</script>

<script setup lang="ts">
import { ref, watch } from "vue";

let selectedGeoDBCities = ref([]);
let cityInput = ref("");
let loadingCities = ref(false);
let isCityListShow = ref(true);

const emit = defineEmits(["cityChange"]);

function debounce(func: any, ms: number) {
  let timeout: number;
  return function () {
    clearTimeout(timeout);
    // eslint-disable-next-line
    // @ts-ignore
    timeout = setTimeout(() => func.apply(this, arguments), ms);
  };
}

async function searchFromGeoDB(newCityInput: string) {
  try {
    console.log("newCityInput", newCityInput);

    loadingCities.value = true;

    const response = await fetch(
      `${GEO_API_URL}/cities?minPopulation=1000000&namePrefix=${newCityInput}`,
      geoOptions
    );

    const cities = await response.json();
    console.log("cities", cities);

    selectedGeoDBCities = cities.data;

    loadingCities.value = false;
  } catch (error) {
    console.log("error during fetch cities", error);
  }
}

const searchFromGeoDBDebounced = debounce(searchFromGeoDB, 1200);

// eslint-disable-next-line
// @ts-ignore
watch(cityInput, (...args) => searchFromGeoDBDebounced(...args));

// const searchFromGeoDB = async (newCityInput: string) => {
//   try {
//     loadingCities.value = true;

//     const response = await fetch(
//       `${GEO_API_URL}/cities?minPopulation=1000000&namePrefix=${newCityInput}`,
//       geoOptions
//     );

//     const cities = await response.json();
//     console.log("cities", cities);

//     selectedGeoDBCities = cities.data;

//     loadingCities.value = false;
//   } catch (error) {
//     console.log("error during fetch cities", error);
//   }
// };
</script>

<style scoped>
#search-city-input {
  width: 100%;
  font-size: large;
  padding: 5px 0px 5px 10px;
  font-weight: bold;
  box-sizing: border-box;
  border: none;
  border: 1px solid #000;
  border-radius: 5px;
}

#selected-cities-array {
  max-width: 100%;
  padding: auto;
  background-color: #fff;
  font-size: large;
  border-radius: 5px;
}

.selected-city {
  text-align: left;
  cursor: pointer;
  padding: 5px;
  padding-left: 10px;
}

ul {
  margin-top: 5px;
  padding-inline-start: 0px;
  list-style-type: none;
}
</style>
