<script setup>
const model = defineModel();
defineProps(["title"]);
</script>

<template>
  <div class="flex gap-2 p-4">
    <input v-model="model" type="checkbox" />
    <span>{{ title }}</span>
  </div>
</template>