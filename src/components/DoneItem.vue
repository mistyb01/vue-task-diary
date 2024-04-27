<script setup>
import { ArrowUturnLeftIcon, CheckIcon } from "@heroicons/vue/24/outline";
defineProps(["title", "id", "subtasks"]);
const emit = defineEmits(["undoComplete"]);
</script>

<template>
  <div class="flex flex-col">
    <div class="flex gap-2 items-center p-2">
      <button @click="$emit('undoComplete', id)" class="group">
        <span class="group-hover:hidden"><CheckIcon class="w-4 h-4" /></span>
        <span class="group-hover:block hidden">
          <ArrowUturnLeftIcon class="w-4 h-4" />
        </span>
      </button>
      <span>{{ title }}</span>
    </div>
    <ul v-if="subtasks.length > 0" class="pl-8">
        <li v-for="item in subtasks" class="flex gap-2 items-center p-2">
          <span v-if="item.completed"><CheckIcon class="w-3 h-3" /></span>
          <span v-else class="border border-gray-400 w-3 h-3"></span>
          {{ item.title }}
        </li>
      </ul>
  </div>
</template>