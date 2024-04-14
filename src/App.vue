<script setup>
import { ref } from "vue";
import TodoItem from "./components/TodoItem.vue";
import DoneItem from "./components/DoneItem.vue";
import TodoContainer from "./components/TodoContainer.vue";
import DoneContainer from "./components/DoneContainer.vue";
import TodoInput from "./components/TodoInput.vue";

const tasks = ref([
  { id: 1, title: "Laundry", isComplete: true },
  { id: 2, title: "Draw for 30 minutes", isComplete: false },
  { id: 3, title: "Code a todo app in Vue", isComplete: false },
  { id: 3, title: "Wash face", isComplete: true },
]);
</script>

<template>
  <div class="container mx-auto p-4 max-w-screen-md">
    <header class="my-8">
      <h1 class="text-2xl">Silly little tasks.</h1>
    </header>

    <main>
      <TodoInput />
      <TodoContainer>
        <TodoItem
          v-for="task in tasks.filter((t) => !t.isComplete)"
          :key="task.id"
          :title="task.title"
        />
      </TodoContainer>
      <DoneContainer>
        <h2 class="text-xl">Completed.</h2>
        <TodoContainer>
          <DoneItem
            v-for="task in tasks.filter((t) => t.isComplete)"
            :key="task.id"
            :title="task.title"
          />
        </TodoContainer>
      </DoneContainer>
    </main>
  </div>
</template>

<style scoped>
</style>
