<script setup>
import { ref, computed } from "vue";
import TodoItem from "./components/TodoItem.vue";
import DoneItem from "./components/DoneItem.vue";
import TodoContainer from "./components/TodoContainer.vue";
import DoneContainer from "./components/DoneContainer.vue";
import TodoInput from "./components/TodoInput.vue";

let id = 0;

const tasks = ref([{ id: id++, title: "sleep", done: true }]);

const newTodoTitle = ref("");

function addTodo() {
  const todoObj = { id: id++, title: newTodoTitle.value, done: false };
  tasks.value.push(todoObj);
  newTodoTitle.value = "";
}

const hasCompleted = computed(
  () => tasks.value.filter((t) => t.done).length > 0
);
const hasIncomplete = computed(
  () => tasks.value.filter((t) => !t.done).length > 0
);
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
        <span v-if="!hasIncomplete">Let's do something! Add a task.</span>
      </TodoContainer>
      <DoneContainer v-if="hasCompleted">
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
