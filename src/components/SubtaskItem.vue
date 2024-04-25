<script setup>
import { ref } from "vue";

const isEditing = ref(true);
const title = ref("subtask")

</script>

<template>
  <div class="flex gap-2 p-4">
    <div class="pl-8">
        <input
          v-if="isEditing"
          placeholder="new subtask"
          type="text"
          class="bg-transparent border-dotted border-2 border-gray-400"
        />
      </div>
  </div>
</template>