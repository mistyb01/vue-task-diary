<script setup>
const model = defineModel();
const emit = defineEmits(["submitTodo"]);

function submit() {
  emit("submitTodo");
}
</script>

<template>
  <form @submit.prevent="submit">
    <div class="flex min-w-min rounded border-solid border-2 border-gray-400">
      <input v-model="model" class="p-2 flex-grow" type="text" />
      <button class="bg-gray-100 w-16" type="submit">+</button>
    </div>
  </form>
</template>