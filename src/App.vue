<script setup>
import { ref } from "vue";
import TodoItem from "./components/TodoItem.vue";
import DoneItem from "./components/DoneItem.vue";
import TodoContainer from "./components/TodoContainer.vue";
import DoneContainer from "./components/DoneContainer.vue";
import TodoInput from "./components/TodoInput.vue";

let id = 0;

const tasks = ref([
  { id: id++, title: "Laundry", done: true },
  { id: id++, title: "Draw for 30 minutes", done: false },
  { id: id++, title: "Code a todo app in Vue", done: false },
  { id: id++, title: "Wash face", done: true },
]);

const newTodoTitle = ref("");

function addTodo() {
  const todoObj = { id: id++, title: newTodoTitle.value, done: false };
  tasks.value.push(todoObj);
  newTodoTitle.value = "";
}
</script>

<template>
  <div class="container mx-auto p-4 max-w-screen-md">
    <header class="my-8">
      <h1 class="text-2xl">Silly little tasks.</h1>
    </header>

    <main>
      <TodoInput v-model="newTodoTitle" @submitTodo="addTodo" />
      <TodoContainer>
        <TodoItem
          v-for="task in tasks.filter((t) => !t.done)"
          :key="task.id"
          :title="task.title"
        />
      </TodoContainer>
      <DoneContainer>
        <h2 class="text-xl">Completed.</h2>
        <TodoContainer>
          <DoneItem
            v-for="task in tasks.filter((t) => t.done)"
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
