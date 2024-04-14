<script setup>
import { TrashIcon, PencilIcon } from "@heroicons/vue/24/outline";
defineProps(["title", "id"]);
const model = defineModel();
const emit = defineEmits(["deleteTodo"]);
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
      <button>
        <PencilIcon class="h-6 w-6" />
      </button>
      <button @click="$emit('deleteTodo', id)">
        <TrashIcon class="h-6 w-6" />
      </button>
    </div>
  </div>
</template>