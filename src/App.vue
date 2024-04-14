<script setup>
import { ref } from "vue";
import TodoItem from "./components/TodoItem.vue";
import TodoContainer from "./components/TodoContainer.vue";
import TodoInput from "./components/TodoInput.vue";

const tasks = ref([
  { id: 1, title: "Laundry", isComplete: false },
  { id: 2, title: "Draw for 30 minutes", isComplete: false },
  { id: 3, title: "Code a todo app in Vue", isComplete: false },
]);
</script>

<template>
  <div class="container mx-auto p-4 max-w-screen-md">
    <header class="mb-8">
      <h1 class="text-2xl">Simply Done!</h1>
    </header>

    <main>
      <TodoContainer>
        <TodoInput />
        <TodoItem
          v-for="task in tasks"
          :key="task.id"
          :title="task.title"
          :isComplete="true"
        />
      </TodoContainer>
    </main>
  </div>
</template>

<style scoped>
</style>
