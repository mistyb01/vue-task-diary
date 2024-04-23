<script setup>
defineProps(["msg"]);
</script>

<template>
  <div class="flex flex-col items-center text-gray-500 gap-4 py-8">
    <img src="@/assets/cat_study.png" width="200"/>
    <span>{{ msg }}</span>
  </div>
</template>