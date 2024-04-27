<script setup>
import { ref } from "vue";
const title = ref("")

const emit = defineEmits(["addSubtask", "closeInput"]);
</script>

<template>
  <div class="flex gap-2 pt-4">
    <div class="pl-4 flex gap-2">
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
            title = ''
            }"
          class="text-pink-500 border border-1 border-pink-500 px-2"><span aria-hidden="true">+ </span>add</button>
          <button 
          type="submit"
          @click="$emit('closeInput')"
          class="text-gray-400 border border-1 border-gray-400 px-2"><span aria-hidden="true">&times; </span>close</button>
      </div>
  </div>
</template>