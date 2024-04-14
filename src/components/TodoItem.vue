<script setup>
import { TrashIcon, PencilIcon } from "@heroicons/vue/24/outline";

const model = defineModel();
defineProps(["title"]);
</script>

<template>
  <div class="flex justify-between p-4">
    <div class="flex gap-4">
      <input v-model="model" type="checkbox" />
      <span>{{ title }}</span>
    </div>
    <div class="flex gap-4">
      <PencilIcon class="h-6 w-6" />
      <TrashIcon class="h-6 w-6" />
    </div>
  </div>
</template>