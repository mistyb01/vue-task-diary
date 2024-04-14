<script setup>
const model = defineModel();
defineProps(["title", "id"]);
const emit = defineEmits(["check"]);

function handleCheck() {
  emit("check", id);
}
</script>

<template>
  <div class="flex gap-2 p-4">
    <input v-model="model" type="checkbox" name="" id="" @click="handleCheck" />
    <span>{{ title }}</span>
  </div>
</template>