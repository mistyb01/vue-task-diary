<script setup>
import { ref } from "vue";
import {
  TrashIcon,
  PencilIcon,
  CheckIcon,
  XMarkIcon,
} from "@heroicons/vue/24/outline";
import SubtaskIcon from "../assets/SubtaskIcon.vue"
import SubtaskInput from "./SubtaskInput.vue"
import SubtaskItem from "./SubtaskItem.vue"

const emit = defineEmits(["checkTodo", "deleteTodo", "submitEdit", "addSubtask"]);

defineProps(["title", "id", "subtasks"]);

const isEditing = ref(false);
const editedTitle = ref("");
const isAddingSubtask = ref(false);

function toggleEditMode(title) {
  isEditing.value = !isEditing.value;
  editedTitle.value = title;
}

function toggleSubtaskInput() {
  isAddingSubtask.value = !isAddingSubtask.value;
}

const iconStyles = "h-5 w-5 hover:text-pink-500";
</script>

<template>
  <div class="rounded hover:bg-pink-50 transition-colors duration-150 p-4">
    <div
      class="flex justify-between"
    >
      <div class="flex gap-4">
        <input @click="$emit('checkTodo', id)" type="checkbox" />
        <div>
          <span v-if="!isEditing">{{ title }}</span>
          <input
            v-else
            v-model="editedTitle"
            type="text"
            class="bg-transparent border-dotted border-2 border-gray-400"
          />
        </div>
      </div>
      <div class="flex gap-4">
        <button
          v-if="isEditing"
          @click="
            () => {
              $emit('submitEdit', id, editedTitle);
              isEditing = false;
            }
          "
        >
          <CheckIcon :class="iconStyles" />
        </button>
        <button v-if="!isEditing" @click="() => toggleSubtaskInput()">
          <SubtaskIcon />
        </button>
        <button @click="() => toggleEditMode(title)">
          <PencilIcon v-if="!isEditing" :class="iconStyles" />
          <XMarkIcon v-else :class="iconStyles" />
        </button>
        <button v-if="!isEditing" @click="$emit('deleteTodo', id)">
          <TrashIcon :class="iconStyles" />
        </button>
      </div>
    </div>
    <div v-if="isAddingSubtask">
      <SubtaskInput @addSubtask="(title)=>emit('addSubtask', id, title)"/>
    </div>
    <div 
      v-if="subtasks.length > 0"
      class="pl-8 mt-2">
      <SubtaskItem
        v-for="subtask in subtasks"
        v-model="subtask.completed"
        :key="subtask.id"
        :title="subtask.title"
        :id="subtask.id"
        :editModeOn="isEditing"
      />
    </div>
  </div>
</template>