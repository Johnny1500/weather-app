<template>
  <SearchCity @city-change="handleSelectedCity" />
  <div>{{ selectedCity }}</div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import SearchCity from "./components/SearchCity.vue";

export default defineComponent({
  name: "App",
  components: {
    SearchCity,
  },
  data() {
    return {
      selectedCity: "",
    };
  },
  methods: {
    handleSelectedCity(selectedCity: string) {
      this.selectedCity = selectedCity;
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
