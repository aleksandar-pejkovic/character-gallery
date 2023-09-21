<template>
    <div class="border rounded-3xl m-5 p-5 flex flex-col items-center">
        <img class="rounded-3xl w-80" :src="character.image" :alt="character.name">
        <h2 class="text-lg font-semibold">{{ character.name }}</h2>
        <p>{{ character.species }}</p>
        <p>Status: {{ character.status }}</p>
        <p>Created: {{ character.created }}</p>
        <p>Location: {{ character.location.name }}</p>
        <p>Origin: {{ character.origin.name }}</p>
    </div>
</template>

<script setup>
const props = defineProps({
    character: Object
})
</script>