<script setup>
import { PlusIcon } from "@heroicons/vue/24/outline";

const model = defineModel();
const emit = defineEmits(["submitTodo"]);

function submit() {
  emit("submitTodo");
}
</script>

<template>
  <form @submit.prevent="submit">
    <div
      class="flex gap-1 min-w-min rounded border-solid border-2 border-gray-400"
    >
      <input
        v-model="model"
        class="p-2 flex-grow"
        type="text"
        placeholder="what do you wanna do?"
      />
      <button
        class="bg-pink-50 w-16 flex justify-center items-center"
        type="submit"
      >
        <PlusIcon class="w-5 h-5" />
      </button>
    </div>
  </form>
</template>