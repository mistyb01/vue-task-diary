<script setup>
import { computed } from "vue";
import { useStorage } from "@vueuse/core";
import LayoutContainer from "./components/LayoutContainer.vue";
import DoneItem from "./components/DoneItem.vue";
import EmptyMessage from "./components/EmptyMessage.vue";
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

const hasCompleted = computed(() => completedTasks.value.length > 0);

// TODO: try to reduce redundancy, this function is also used in Todo.vue
function undoComplete(todoId) {
  const index = tasks.value.findIndex((t) => t.id === todoId);
  tasks.value[index].completionDate = null;
}
</script>

<template>
  <LayoutContainer>    
    <header class="my-8">
      <h1 class="font-semibold text-2xl text-pink-500">History</h1>
    </header>

    <main>
      <div v-if="hasCompleted" class="flex flex-col gap-8">
        <div v-for="dateGroup in tasksByDateArray" :key="dateGroup.date">
          <h3 class="font-bold">
            {{ dateGroup.date.replaceAll('/','.') }}
          </h3>        
            <DoneItem 
              v-for="task in dateGroup.tasks" 
              :key="task.id" 
              :title="task.title" 
              :id="task.id" 
              :subtasks="task.subtasks"
              class="list-disc"
              @undoComplete="(todoId) => undoComplete(todoId)"
            />
            <hr class="w-24 h-0.5 my-8 bg-pink-100 border-0 rounded md:my-10">
        </div>
      </div>
      <EmptyMessage v-else msg="Nothing yet!"/>
      
    </main>
  </LayoutContainer>
</template>

<style scoped>
</style>
