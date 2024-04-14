<script setup>
import { TrashIcon, PencilIcon } from "@heroicons/vue/24/outline";

const model = defineModel();
defineProps(["title"]);
</script>

<template>
  <div
    class="group flex justify-between p-4 rounded hover:bg-gray-100 transition-colors duration-150"
  >
    <div class="flex gap-4">
      <input v-model="model" type="checkbox" />
      <span>{{ title }}</span>
    </div>
    <div
      class="opacity-0 group-hover:opacity-100 transition-opacity duration-150 flex gap-4"
    >
      <PencilIcon class="h-6 w-6" />
      <TrashIcon class="h-6 w-6" />
    </div>
  </div>
</template>