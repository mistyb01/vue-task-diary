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

// function below was gpt-assisted
// converts a yyyy-mm-dd string into a string with abbreviated month and day.
function formatDate(inputDate) {
    // Parse the input date string
    const parts = inputDate.split('-');
    const year = parseInt(parts[0]);
    const month = parseInt(parts[1], 10);
    const day = parseInt(parts[2], 10);

    // Create a Date object
    const date = new Date(year, month - 1, day);

    // Get the month name
    const monthNames = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
    const monthName = monthNames[date.getMonth()];

    let formattedString = monthName + ' ' + day;
  
    // only append the year if it is not the current one
    const currentYear = new Date().getFullYear();
    if (year !== currentYear) {
      formattedString = formattedString + ', ' + year
    } 

    return formattedString;
}

function groupTasksByDate(taskArray) {
  let tasksByDateObj = {}
  taskArray.forEach((task) => {
      // date field is converted to yyyy-mm-dd string
      let dateStr = new Date(task.completionDate).toISOString().substring(0,10);
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
    
    let tasksByDateArrDateDescending = tasksByDateArr.sort((a,b) => a.date < b.date);
    console.log(tasksByDateArrDateDescending)
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
          <h3 class="font-bold capitalize">
            {{ dateGroup.date === todayString ? 'today' : 
            dateGroup.date === yesterdayString ? 'yesterday' :
            formatDate(dateGroup.date)}}
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
