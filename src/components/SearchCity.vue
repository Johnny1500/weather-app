<template>
  <input
    type="text"
    placeholder="Search for city"
    v-model="cityInput"
    id="search-city"
  />
  <div id="selected-cities-array" v-show="cityInput.length > 0">
    <ul v-if="!loadingCities">
      <li
        v-for="item in selectedGeoDBCities"
        :key="item['id']"
        class="selected-city"
        value="item['name']"
        @click="handleSelectedSearchCity(item['name'])"
      >
        {{ item["name"] }}
      </li>
    </ul>
    <div v-else>Loading...</div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { debounce } from "lodash";
import { geoOptions, GEO_API_URL } from "../api";

export default defineComponent({
  name: "SearchCity",
  data() {
    return {
      selectedGeoDBCities: [],
      cityInput: "",
      loadingCities: false,
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
    this.searchFromGeoDB = debounce(async (newCityInput) => {
      try {
        this.loadingCities = true;

        const response = await fetch(
          `${GEO_API_URL}/cities?minPopulation=1000000&namePrefix=${newCityInput}`,
          geoOptions
        );
        const cities = await response.json();
        console.log("cities", cities);

        this.selectedGeoDBCities = cities.data;

        this.loadingCities = false;
      } catch (error) {
        console.log("error during fetch cities", error);
      }
    }, 1200);
  },
  beforeUnmount() {
    // eslint-disable-next-line
    // @ts-ignore
    this.searchFromGeoDB.cancel();
  },
});
</script>

<script setup lang="ts">
const emit = defineEmits(["cityChange"]);

const handleSelectedSearchCity = (city: string) => {
  emit("cityChange", city);
};
</script>

<style scoped>
#search-city {
  width: 100%;
  font-size: large;
  padding: 5px 0px 5px 10px;
  font-weight: bold;
  box-sizing: border-box;
  border: none;
  border: 1px solid #000;
  border-radius: 3px;
}

#selected-cities-array {
  max-width: 100%;
  padding: auto;
  background-color: #fff;
  font-size: large;
}

.selected-city {
  cursor: pointer;
  padding: 5px;
  border-bottom: 1px solid;
}

ul {
  margin-top: 5px;
  padding-inline-start: 0px;
  list-style-type: none;
}
</style>
