<template>
    <div ref="scrollComponent" class="flex flex-wrap justify-around overflow-y-auto max-h-screen">
        <h1 class="text-center p-10 text-5xl font-bold text-red-700 w-full">Characters from Rick and Morty</h1>
        <Character v-for="character in characters" :key="character.id" :character="character" />
    </div>
</template>
  
<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import axios from 'axios';
import Character from './Character.vue';

const scrollComponent = ref(null);
const characters = ref([]);
const info = ref(null);
const initialUri = ref('https://rickandmortyapi.com/api/character');
const nextPageNumber = ref(1)

const fetchData = (uri) => {
    nextPageNumber.value++
    axios
        .get(uri)
        .then((response) => {
            characters.value.push(...response.data.results);
            info.value = response.data.info;
        })
        .catch((error) => {
            console.error('Error fetching data:', error);
        });
};

const fetchMoreData = () => {
    const hasNextPage = info.value && info.value.next
    if (hasNextPage) {
        const url = new URL(info.value.next);
        const queryParams = new URLSearchParams(url.search);
        const urlPageNumber = queryParams.get('page');
        const nextPageIsNotLoaded = nextPageNumber.value == urlPageNumber;
        if (nextPageIsNotLoaded) {
            fetchData(info.value.next);
        }
    }
};

onMounted(() => {
    fetchData(initialUri.value);
    scrollComponent.value.addEventListener('scroll', handleScroll)
});

onUnmounted(() => {
    scrollComponent.value.removeEventListener('scroll', handleScroll)
})

let previousScrollTop = 0;

const handleScroll = (e) => {
    let containerElement = scrollComponent.value;
    const scrollTop = containerElement.scrollTop;
    const scrollHeight = containerElement.scrollHeight;
    const clientHeight = containerElement.clientHeight;
    const scrollDifference = scrollTop - previousScrollTop;
    const bottomHeight = scrollTop + clientHeight;
    const customScrollLineOffset = scrollHeight - 300;
    const isScrollingDown = scrollDifference > 0;

    if (bottomHeight >= customScrollLineOffset && isScrollingDown) {
        fetchMoreData();
    }
    previousScrollTop = scrollTop;
};
</script>
  