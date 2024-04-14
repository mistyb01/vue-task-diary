<script setup>
import { ref } from "vue";

const newTodoTitle = ref("");
const emit = defineEmits(["response"]);
</script>

<template>
  <div class="flex min-w-min rounded border-solid border-2 border-gray-400">
    <input v-model="newTodoTitle" class="p-2 flex-grow" type="text" />
    <button class="bg-gray-100 w-16" @click="$emit('response', newTodoTitle)">
      +
    </button>
  </div>
</template>