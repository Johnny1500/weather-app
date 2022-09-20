<template>
  <div>
    {{ msg }}
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "SearchCity",
});
</script>

<script setup lang="ts">
const msg = "Hello weather app!";
</script>

<style scoped></style>
