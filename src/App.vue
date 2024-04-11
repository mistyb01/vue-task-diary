<script setup>
import { ref } from "vue";
import TodoItem from "./components/TodoItem.vue";

const tasks = ref([
  { id: 1, title: "Laundry", isComplete: false },
  { id: 2, title: "Draw for 30 minutes", isComplete: false },
  { id: 3, title: "Code a todo app in Vue", isComplete: false },
]);
</script>

<template>
  <header>
    <h1>DO IT!!!!</h1>
  </header>

  <main>
    <!-- : is short for v-bind: -->
    <TodoItem
      v-for="task in tasks"
      :key="task.id"
      :title="task.title"
      :isComplete="true"
    />
  </main>
</template>

<style scoped>
</style>
