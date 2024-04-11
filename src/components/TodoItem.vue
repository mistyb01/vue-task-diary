<script setup>
defineProps(["title", "isChecked"]);
</script>

<template>
  <div>
    <input type="checkbox" name="" id="" v-bind:checked="isChecked" />
    <span>{{ title }}</span>
  </div>
</template>