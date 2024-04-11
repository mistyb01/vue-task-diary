<script setup>
import TodoItem from "./components/TodoItem.vue";
</script>

<template>
  <header>
    <h1>DO IT!!!!</h1>
  </header>

  <main>
    <TodoItem title="Laundry" v-bind:isChecked="true" />
  </main>
</template>

<style scoped>
</style>
