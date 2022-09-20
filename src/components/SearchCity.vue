<template>
  <div>
    <input type="text" placeholder="Search for city" @input="handleChange" />
  </div>
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
  emit("cityChange", event.target.value);
};
</script>

<style scoped></style>
