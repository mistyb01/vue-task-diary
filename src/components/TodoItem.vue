<script setup>
defineProps(["title", "isChecked"]);
</script>

<template>
  <div>
    <input type="checkbox" name="" id="" v-bind:isComplete="isComplete" />
    <span class="text-xl">{{ title }}</span>
  </div>
</template>