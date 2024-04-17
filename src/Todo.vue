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
  {
    id: uuidv4(),
    title: "example task",
    done: false,
    creationDate: Date.now(),
    completionDate: null,
  },
]);

const newTodoTitle = ref("");

function addTodo() {
  const todoObj = {
    id: uuidv4(),
    title: newTodoTitle.value,
    done: false,
    creationDate: Date.now(),
    completionDate: null,
  };
  tasks.value.push(todoObj);
  newTodoTitle.value = "";
}

function deleteTodo(todoId) {
  tasks.value = tasks.value.filter((t) => t.id !== todoId);
}

function undoComplete(todoId) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].done = false;
  tasks.value[index].completionDate = null;
}

function editTodo(todoId, editedTitle) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].title = editedTitle;
}

function checkTodo(todoId) {
  // note that each tasks 'done' state is updated automatically by the input's model.
  // this function just assigns a completion date.
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].completionDate = Date.now();
}

const incompleteTasks = computed(() => tasks.value.filter((t) => !t.done));
const completedTasks = computed(() => tasks.value.filter((t) => t.done));

const motivationalHeadings = [
  "Seize the day.",
  "Start today.",
  "Do something for future you!",
  "Knock it out!",
];

function randomIntFromInterval(min, max) {
  // min and max included
  return Math.floor(Math.random() * (max - min + 1) + min);
}

const randomIndex = randomIntFromInterval(0, motivationalHeadings.length - 1);
const headingText = motivationalHeadings[randomIndex];

</script>

<template>
  <div class="container mx-auto p-4 max-w-screen-sm min-h-screen">
    <header class="my-8">
      <h1 class="font-semibold text-2xl text-pink-500">{{ headingText }}</h1>
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
          @checkTodo="(todoId) => checkTodo(todoId)"
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
      <div class="mt-8">
        <a href="#/history" class="text-pink-400">view history</a>
      </div>
    </main>
  </div>
</template>

<style scoped>
</style>
