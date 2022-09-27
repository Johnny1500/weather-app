<template>
  <input
    type="text"
    placeholder="Search for city"
    @input="handleChange"
    id="search-city"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "SearchCity",
});
</script>

<script setup lang="ts">
const emit = defineEmits(["cityChange"]);

const handleChange = (event: Event) => {
  emit("cityChange", (event.target as HTMLInputElement).value);
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
</style>
