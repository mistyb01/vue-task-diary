<script setup>
import { ref, computed } from "vue";
import TodoItem from "./components/TodoItem.vue";
import DoneItem from "./components/DoneItem.vue";
import TodoContainer from "./components/TodoContainer.vue";
import DoneContainer from "./components/DoneContainer.vue";
import TodoInput from "./components/TodoInput.vue";
import { v4 as uuidv4 } from "uuid";
import { useStorage } from "@vueuse/core";

const tasks = useStorage("task-store", [
  { id: uuidv4(), title: "example task", done: false },
]);

const newTodoTitle = ref("");

function addTodo() {
  const todoObj = { id: uuidv4(), title: newTodoTitle.value, done: false };
  tasks.value.push(todoObj);
  newTodoTitle.value = "";
}

function deleteTodo(todoId) {
  tasks.value = tasks.value.filter((t) => t.id !== todoId);
}

function undoComplete(todoId) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].done = false;
}

function editTodo(todoId, editedTitle) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].title = editedTitle;
}

const incompleteTasks = computed(() => tasks.value.filter((t) => !t.done));
const completedTasks = computed(() => tasks.value.filter((t) => t.done));
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
          v-for="task in incompleteTasks"
          v-model="task.done"
          :key="task.id"
          :id="task.id"
          :title="task.title"
          @deleteTodo="(todoId) => deleteTodo(todoId)"
          @submitEdit="(todoId, editedTitle) => editTodo(todoId, editedTitle)"
        />
        <span v-if="!incompleteTasks.length"
          >Let's do something! Add a task.</span
        >
      </TodoContainer>
      <DoneContainer v-if="completedTasks.length">
        <h2 class="text-xl">Completed!</h2>
        <TodoContainer>
          <DoneItem
            v-for="task in completedTasks"
            :key="task.id"
            :id="task.id"
            :title="task.title"
            @undoComplete="(todoId) => undoComplete(todoId)"
          />
        </TodoContainer>
      </DoneContainer>
    </main>
  </div>
</template>

<style scoped>
</style>
