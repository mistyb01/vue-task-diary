<script setup>
import { ref, computed } from "vue";
import Todo from "./Todo.vue";
import History from "./History.vue";
import Preferences from "./Preferences.vue";

const routes = {
  "/": Todo,
  "/history": History,
  "/preferences": Preferences
};

const currentPath = ref(window.location.hash);

window.addEventListener("hashchange", () => {
  currentPath.value = window.location.hash;
});

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || "/"] || NotFound;
});
</script>

<template>
  <Navbar/>
  <component :is="currentView" />
</template>