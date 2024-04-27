<script setup>
defineProps(["msg"]);
</script>

<template>
  <div class="flex flex-col items-center text-gray-500 gap-4 py-8">
    <span>{{ msg }}</span>
  </div>
</template>