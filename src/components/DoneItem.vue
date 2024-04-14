<script setup>
defineProps(["title"]);
</script>

<template>
  <div class="flex gap-2 p-4">
    <span>&#x2713;</span>
    <span>{{ title }}</span>
  </div>
</template>