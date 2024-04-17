<script setup>
import { computed } from "vue";
import { useStorage } from "@vueuse/core";

const tasks = useStorage("task-store", []);
const completedTasks = computed(() => tasks.value.filter((t) => t.done));

let tasksByDateObj = {}
completedTasks.value.forEach((task) => {
  let dateObj = new Date(task.completionDate);
  let dateStr = dateObj.toLocaleDateString();
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
    <header class="my-8">
      <h1 class="font-semibold text-2xl text-pink-500">History</h1>
    </header>

    <main>
      <div>
        <ul>
          <li v-for="dateGroup in tasksByDateArray" :key="dateGroup.date">
            <p class="font-bold">
              {{ dateGroup.date }}
              </p>
              <ul>
                <li v-for="task in dateGroup.tasks" :key="task.id" class="list-disc">
                {{ task.title }}
                </li>
              </ul>
            
          </li>
        </ul>
      </div>
      <div class="mt-4">
        <a href="#/">go back</a>
      </div>
    </main>
  </div>
</template>

<style scoped>
</style>
