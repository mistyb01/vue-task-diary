<script setup>
import { computed } from "vue";
import { useStorage } from "@vueuse/core";
import LayoutContainer from "./components/LayoutContainer.vue";
import DoneItem from "./components/DoneItem.vue";
import EmptyMessage from "./components/EmptyMessage.vue";

const completedTasks = useStorage("completed-task-store", []);
const todos = useStorage("pending-task-store", []);

const todayString = new Date().toISOString().substring(0,10);

let yesterday = new Date();
yesterday.setDate(yesterday.getDate() - 1);
const yesterdayString = yesterday.toISOString().substring(0,10);

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
    
    let tasksByDateArrDateDescending = tasksByDateArr.sort((a,b) => b.date - a.date);
    return tasksByDateArrDateDescending;
}

const tasksByDateArray = computed(() => groupTasksByDate(completedTasks.value));

const hasCompleted = computed(() => completedTasks.value.length > 0);

// TODO: try to reduce redundancy, this function is also used in Todo.vue
function undoComplete(todoId) {
  const index = completedTasks.value.findIndex((t) => t.id === todoId);
  let currentTodo = completedTasks.value[index];
  currentTodo.completionDate = null;

  todos.value.push(currentTodo);
  completedTasks.value = completedTasks.value.filter((t) => t.id !== todoId);
}
</script>

<template>
  <LayoutContainer headingText="History">    
    <main>
      <div v-if="hasCompleted" class="flex flex-col gap-8">
        <div v-for="dateGroup in tasksByDateArray" :key="dateGroup.date">
          <h3 class="font-bold">
            {{ dateGroup.date === todayString ? 'Today' : 
            dateGroup.date === yesterdayString ? 'Yesterday' :
            dateGroup.date.replaceAll('/','.') }}
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
        </div>
      </div>
      <EmptyMessage v-else msg="Nothing yet!"/>
      
    </main>
  </LayoutContainer>
</template>

<style scoped>
</style>
