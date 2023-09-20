<template>
    <div class="flex flex-wrap">
        <div v-for="character in characters" :key="character.id" class="w-full sm:w-1/2 md:w-1/4 lg:w-1/4 xl:w-1/4 p-4">
            <div class="border rounded p-4 flex flex-col items-center">
                <img :src="character.image" :alt="character.name">
                <h2 class="text-lg font-semibold">{{ character.name }}</h2>
                <p>{{ character.species }}</p>
                <p>Status: {{ character.status }}</p>
                <p>Created: {{ character.created }}</p>
                <p>Location: {{ character.location.name }}</p>
                <p>Origin: {{ character.origin.name }}</p>
            </div>
        </div>
    </div>
</template>
  
<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const characters = ref([]);

const fetchData = () => {
    axios
        .get('https://rickandmortyapi.com/api/character')
        .then((response) => {
            characters.value = response.data.results;
        })
        .catch((error) => {
            console.error('Error fetching data:', error);
        });
};

onMounted(() => {
    fetchData();
});
</script>
  