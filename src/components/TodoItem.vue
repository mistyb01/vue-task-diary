<script setup>
import { ref, toRefs } from "vue";
import {
  TrashIcon,
  PencilIcon,
  CheckIcon,
  XMarkIcon,
} from "@heroicons/vue/24/outline";
const model = defineModel();
const emit = defineEmits(["deleteTodo", "submitEdit"]);

defineProps(["title", "id"]);

const isEditing = ref(false);
const editedTitle = ref("");

function toggleEditMode(title) {
  isEditing.value = !isEditing.value;
  editedTitle.value = title;
}
</script>

<template>
  <div
    class="flex justify-between p-4 rounded hover:bg-gray-100 transition-colors duration-150"
  >
    <div class="flex gap-4">
      <input v-model="model" type="checkbox" />
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
        <CheckIcon class="h-6 w-6" />
      </button>
      <button @click="() => toggleEditMode(title)">
        <PencilIcon v-if="!isEditing" class="h-6 w-6" />
        <XMarkIcon v-else class="h-6 w-6" />
      </button>
      <button v-if="!isEditing" @click="$emit('deleteTodo', id)">
        <TrashIcon class="h-6 w-6" />
      </button>
    </div>
  </div>
</template>