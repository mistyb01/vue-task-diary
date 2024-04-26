<script setup>
import { ref } from "vue";

const emit = defineEmits(["checkTodo"]);

defineProps(["title", "id"]);
</script>

<template>
  <div
    class="flex justify-between p-4 rounded hover:bg-pink-50 transition-colors duration-150"
  >
    <div class="flex gap-4">
      <input @click="$emit('checkTodo', id)" type="checkbox" />
      <div>
        <span>{{ title }}</span>
      </div>
    </div>
  </div>

</template>