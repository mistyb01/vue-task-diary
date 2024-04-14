<script setup>
defineProps(["title", "isChecked"]);
</script>

<template>
  <div class="flex gap-2">
    <input type="checkbox" name="" id="" v-bind:isComplete="isComplete" />
    <span>{{ title }}</span>
  </div>
</template>