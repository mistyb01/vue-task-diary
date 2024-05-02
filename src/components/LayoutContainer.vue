<script setup>
defineProps(["headingText"])
</script>

<template>
    <div class="container mx-auto p-4 max-w-screen-sm min-h-screen">
        <!-- nav bar -->
        <div class="flex gap-4 justify-end">
            <a href="#/" class="text-pink-400">todo</a>
            <a href="#/history" class="text-pink-400">history</a>
            <a href="#/preferences" class="text-pink-400">preferences</a>
        </div>
        <!-- page heading -->
        <header class="my-8">
            <h1 class="font-semibold text-2xl text-pink-500">{{ headingText }}</h1>
        </header>
        <slot/>
    </div>
</template>