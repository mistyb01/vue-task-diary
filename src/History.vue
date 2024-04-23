<script setup>
import { computed } from "vue";
import { useStorage } from "@vueuse/core";
import DoneItem from "./components/DoneItem.vue";
import { ArrowLongLeftIcon } from "@heroicons/vue/24/outline";

const tasks = useStorage("task-store", []);
const completedTasks = computed(() => tasks.value.filter((t) => t.completionDate));


function groupTasksByDate(taskArray) {
  let tasksByDateObj = {}
  taskArray.forEach((task) => {
      let dateStr = task.completionDate;
      if (tasksByDateObj[dateStr]) {
        tasksByDateObj[dateStr].push(task);
      } else {
        tasksByDateObj[dateStr] = [task];
      }
    })
    // convert object to an array
    let tasksByDateArr = Object.keys(tasksByDateObj).map((date) => {
    return {
      date,
      tasks: tasksByDateObj[date]
    }})
    return tasksByDateArr;
}

const tasksByDateArray = computed(() => groupTasksByDate(completedTasks.value));

// TODO: try to reduce redundancy, this function is also used in Todo.vue
function undoComplete(todoId) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].completionDate = null;
}
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
            <DoneItem v-for="task in dateGroup.tasks" :key="task.id" :title="task.title" :id="task.id" class="list-disc"
            @undoComplete="(todoId) => undoComplete(todoId)"
            />
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<style scoped>
</style>
