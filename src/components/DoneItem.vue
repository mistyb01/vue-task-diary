<script setup>
import { ArrowUturnLeftIcon } from "@heroicons/vue/24/outline";
defineProps(["title"]);
</script>

<template>
  <div class="flex gap-2 p-4">
    <button class="group">
      <span class="group-hover:hidden">&#x2713;</span>
      <span class="group-hover:block hidden">
        <ArrowUturnLeftIcon class="w-4 h-4" />
      </span>
    </button>
    <span>{{ title }}</span>
  </div>
</template>