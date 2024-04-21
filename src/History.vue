<script setup>
import { computed } from "vue";
import { useStorage } from "@vueuse/core";
import DoneItem from "./components/DoneItem.vue";
import { ArrowLongLeftIcon } from "@heroicons/vue/24/outline";

const tasks = useStorage("task-store", []);
const completedTasks = computed(() => tasks.value.filter((t) => t.completionDate));

let tasksByDateObj = {}
completedTasks.value.forEach((task) => {
  let dateStr = task.completionDate;
  if (tasksByDateObj[dateStr]) {
    tasksByDateObj[dateStr].push(task);
  } else {
    tasksByDateObj[dateStr] = [task];
  }
})

const tasksByDateArray = Object.keys(tasksByDateObj).map((date) => {
  return {
    date,
    tasks: tasksByDateObj[date]
  }
})

console.log(tasksByDateArray)
</script>

<template>
  <div class="container mx-auto p-4 max-w-screen-sm min-h-screen">
      <a href="#/" class="my-4 text-pink-400 flex gap-2 items-center">
        <ArrowLongLeftIcon class="w-5 h-5"/> back to todo list
      </a>
    
    <header class="my-8">
      <h1 class="font-semibold text-2xl text-pink-500">History</h1>
    </header>

    <main>
      <div class="flex flex-col gap-8">
        <ul v-for="dateGroup in tasksByDateArray" :key="dateGroup.date">
          <h3 class="font-bold">
            {{ dateGroup.date }}
          </h3>        
          <li>
            <DoneItem v-for="task in dateGroup.tasks" :key="task.id" :title="task.title" class="list-disc"/>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<style scoped>
</style>
