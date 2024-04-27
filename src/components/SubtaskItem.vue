<script setup>
import {
  TrashIcon,
  PencilIcon,
} from "@heroicons/vue/24/outline";

const model = defineModel();
defineProps(["title", "editModeOn"]);

const iconStyles = "h-5 w-5 hover:text-pink-500";
</script>

<template>
  <div
    class="flex justify-between py-2 rounded hover:bg-pink-50 transition-colors duration-150"
  >
    <div class="flex gap-4">
      <input v-model="model" type="checkbox" />
      <div>
        <span class="text-gray-500">{{ title }}</span>
      </div>
    </div>
    <div v-if="editModeOn"><TrashIcon :class="iconStyles"/></div>
  </div>

</template>