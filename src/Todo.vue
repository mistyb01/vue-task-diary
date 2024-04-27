<script setup>
import { ref, computed } from "vue";
import TodoItem from "./components/TodoItem.vue";
import DoneItem from "./components/DoneItem.vue";
import TodoContainer from "./components/TodoContainer.vue";
import DoneContainer from "./components/DoneContainer.vue";
import TodoInput from "./components/TodoInput.vue";
import EmptyMessage from "./components/EmptyMessage.vue";

import { v4 as uuidv4 } from "uuid";
import { useStorage } from "@vueuse/core";

const todaysDate = new Date().toLocaleDateString();

const tasks = useStorage("task-store", [
  {
    id: uuidv4(),
    title: "example task",
    creationDate: new Date(),
    completionDate: todaysDate,
    subtasks: []
  },
]);

const newTodoTitle = ref("");

function addTodo() {
  const todoObj = {
    id: uuidv4(),
    title: newTodoTitle.value,
    creationDate: todaysDate,
    completionDate: null,
    subtasks: []
  };
  tasks.value.push(todoObj);
  newTodoTitle.value = "";
}

function deleteTodo(todoId) {
  tasks.value = tasks.value.filter((t) => t.id !== todoId);
}

function undoComplete(todoId) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].completionDate = null;
}

function editTodo(todoId, editedTitle) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].title = editedTitle;
}

function checkTodo(todoId) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].completionDate = todaysDate;
}

function addSubtask(todoId, subtaskTitle) {
  const subtask = {
    id: uuidv4(),
    title: subtaskTitle,
    completed: false
  }
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].subtasks.push(subtask);
}

const incompleteTasks = computed(() => tasks.value.filter((t) => !t.completionDate));
const completedTasks = computed(() => tasks.value.filter((t) => t.completionDate));

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
          :key="task.id"
          :id="task.id"
          :title="task.title"
          :subtasks="task.subtasks"
          @checkTodo="(todoId) => checkTodo(todoId)"
          @deleteTodo="(todoId) => deleteTodo(todoId)"
          @submitEdit="(todoId, editedTitle) => editTodo(todoId, editedTitle)"
          @addSubtask="(todoId, title) => addSubtask(todoId, title)"
          />
        <!-- @addSubtask="(todoId) => addSubtask(todoId)" -->
        <EmptyMessage 
          v-if="!incompleteTasks.length"
          msg="Let's do something! Add a task."
        />
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
