<script setup>
import { ref } from "vue";
import LayoutContainer from "./components/LayoutContainer.vue";
import TodoItem from "./components/TodoItem.vue";
import DoneItem from "./components/DoneItem.vue";
import TodoContainer from "./components/TodoContainer.vue";
import DoneContainer from "./components/DoneContainer.vue";
import TodoInput from "./components/TodoInput.vue";
import EmptyMessage from "./components/EmptyMessage.vue";

import { v4 as uuidv4 } from "uuid";
import { useStorage } from "@vueuse/core";

const todaysDate = new Date().toISOString().substring(0,10);

const completedTasks = useStorage("completed-task-store", [
  {
    id: uuidv4(),
    title: "you can click the checkmark to 'undo'",
    creationDate: new Date(),
    completionDate: todaysDate,
    subtasks: []
  },
]);

const todos = useStorage("pending-task-store", [
  {
    id: uuidv4(),
    title: "here's an example todo",
    creationDate: new Date(),
    completionDate: null,
    subtasks: []
  },
  {
    id: uuidv4(),
    title: "edit me",
    creationDate: new Date(),
    completionDate: null,
    subtasks: []
  },
  {
    id: uuidv4(),
    title: "delete me",
    creationDate: new Date(),
    completionDate: null,
    subtasks: []
  },
  {
    id: uuidv4(),
    title: "break me into smaller chunks! give me a subtask",
    creationDate: new Date(),
    completionDate: null,
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
  todos.value.push(todoObj);
  newTodoTitle.value = "";
}

function deleteTodo(todoId) {
  todos.value = todos.value.filter((t) => t.id !== todoId);
}

function undoComplete(todoId) {
  const index = completedTasks.value.findIndex((t) => t.id === todoId);
  let currentTodo = completedTasks.value[index];
  currentTodo.completionDate = null;

  todos.value.push(currentTodo);
  completedTasks.value = completedTasks.value.filter((t) => t.id !== todoId);
}

function editTodo(todoId, editedTitle) {
  const index = todos.value.findIndex((t) => t.id === todoId);
  todos.value[index].title = editedTitle;
}

function checkTodo(todoId) {
  const index = todos.value.findIndex((t) => t.id === todoId);
  let currentTodo = todos.value[index];
  currentTodo.completionDate = todaysDate;

  completedTasks.value.push(currentTodo);
  todos.value = todos.value.filter((t) => t.id !== todoId);
}

function addSubtask(todoId, subtaskTitle) {
  const subtask = {
    id: uuidv4(),
    title: subtaskTitle,
    completed: false
  }
  const index = todos.value.findIndex((t) => t.id === todoId);
  todos.value[index].subtasks.push(subtask);
}

function deleteSubtask(todoId, subId) {
  const index = todos.value.findIndex((t) => t.id === todoId);
  todos.value[index].subtasks = tasks.value[index].subtasks.filter((s) => s.id !== subId);  
}

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
  <LayoutContainer :headingText="headingText">
    <main>
      <TodoInput v-model="newTodoTitle" @submitTodo="addTodo" />
      <TodoContainer>
        <TodoItem
          v-for="task in todos"
          :key="task.id"
          :id="task.id"
          :title="task.title"
          :subtasks="task.subtasks"
          @checkTodo="(todoId) => checkTodo(todoId)"
          @deleteTodo="(todoId) => deleteTodo(todoId)"
          @submitEdit="(todoId, editedTitle) => editTodo(todoId, editedTitle)"
          @addSubtask="(todoId, title) => addSubtask(todoId, title)"
          @deleteSubtask="(todoId, subId) => deleteSubtask(todoId, subId)"
          />
        <!-- @addSubtask="(todoId) => addSubtask(todoId)" -->
        <EmptyMessage 
          v-if="!todos.length"
          msg="Let's do something! Add a task."
        />
      </TodoContainer>
      <DoneContainer v-if="completedTasks.length">
        <h2 class="text-xl">Completed today</h2>
        <TodoContainer>
          <DoneItem
            v-for="task in completedTasks.filter(t=>t.completionDate === todaysDate)"
            :key="task.id"
            :id="task.id"
            :title="task.title"
            :subtasks="task.subtasks"
            @undoComplete="(todoId) => undoComplete(todoId)"
          />
        </TodoContainer>
      </DoneContainer>
    </main>
  </LayoutContainer>
  </template>

<style scoped>
</style>
