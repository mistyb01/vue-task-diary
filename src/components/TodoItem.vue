<script setup>
import { ref, toRefs } from "vue";
import { TrashIcon, PencilIcon } from "@heroicons/vue/24/outline";
const model = defineModel();
const emit = defineEmits(["deleteTodo"]);

// extra steps to make it possible to use title prop within the script
const props = defineProps(["title", "id"]);
const { title } = toRefs(props);

const isEditing = ref(false);
const editingValue = ref(title.value);

function toggleEdit() {
  isEditing.value = !isEditing.value;
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
          v-model="editingValue"
          type="text"
          class="bg-transparent border-dotted border-2 border-gray-400"
        />
      </div>
    </div>
    <div class="flex gap-4">
      <button @click="toggleEdit">
        <PencilIcon class="h-6 w-6" />
      </button>
      <button @click="$emit('deleteTodo', id)">
        <TrashIcon class="h-6 w-6" />
      </button>
    </div>
  </div>
</template>