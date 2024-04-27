<script setup>
const model = defineModel();
defineProps(["title"]);
</script>

<template>
  <div
    class="flex justify-between p-2 rounded hover:bg-pink-50 transition-colors duration-150"
  >
    <div class="flex gap-4">
      <input v-model="model" type="checkbox" />
      <div>
        <span class="text-gray-500">{{ title }}</span>
      </div>
    </div>
  </div>

</template>