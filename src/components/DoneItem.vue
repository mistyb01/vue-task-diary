<script setup>
import { ref } from 'vue';
import { ArrowUturnLeftIcon, CheckIcon, ChevronRightIcon, ChevronUpIcon } from "@heroicons/vue/24/outline";

defineProps(["task"]);
const emit = defineEmits(["undoComplete"]);
const showSubtasks = ref(false);

</script>

<template>
  <div class="flex flex-col">
    <div class="flex justify-between items-center">
      <div class="flex gap-2 items-center pt-2 ">
        <button @click="$emit('undoComplete', task.id)" class="group">
        <span class="group-hover:hidden"><CheckIcon class="w-4 h-4" /></span>
        <span class="group-hover:block hidden">
          <ArrowUturnLeftIcon class="w-4 h-4" />
        </span>
      </button>
      <span>{{ task.title }}</span>
      <button class="text-xs text-gray-400 rounded uppercase ml-4" v-if="task.subtasks.length > 0"
        @click="()=>showSubtasks = !showSubtasks">
      <span v-if="!showSubtasks" class="flex items-center">
        expand
        <ChevronRightIcon class="w-4 h-4 inline ml-1"/>
      </span>
      <span v-else class="flex items-center">
        hide
        <ChevronUpIcon class="w-4 h-4 inline ml-1"/>
      </span>
    </button>
      </div>
      <span class="text-sm font-light text-gray-400">
        {{ new Date(task.completionDate).toLocaleTimeString([], { hour: "numeric", minute: "2-digit" }) }}
      </span>
    </div>
    <ul v-if="task.subtasks.length > 0 && showSubtasks" class="pl-8">
        <li v-for="item in task.subtasks" class="flex gap-2 items-center">
          <span v-if="item.completed"><CheckIcon class="w-3 h-3" /></span>
          <span v-else class="border border-gray-400 w-3 h-3"></span>
          <span class="text-gray-500 text-sm">
            {{ item.title }}
          </span>
        </li>
      </ul>
  </div>
</template>