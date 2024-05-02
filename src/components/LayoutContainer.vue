<script setup>
import Navbar from "./Navbar.vue";
</script>

<template>
    <div class="container mx-auto p-4 max-w-screen-sm min-h-screen">
        <Navbar/>
        <slot/>
    </div>
</template>