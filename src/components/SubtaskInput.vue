<script setup>
import { ref } from "vue";
const title = ref("")

const emit = defineEmits(["addSubtask"]);
</script>

<template>
  <div class="flex gap-2 p-4">
    <div class="pl-8 flex gap-2">
        <input
          v-model="title"
          placeholder="new subtask"
          type="text"
          class="bg-transparent border-dotted border-2 border-gray-400"
        />
        <button 
          type="submit"
          @click="() => {
            $emit('addSubtask', title)
            title.value = ''
            }"
          class="text-pink-500 border border-1 border-pink-500 px-2"><span aria-hidden="true">+ </span>add</button>
      </div>
  </div>
</template>