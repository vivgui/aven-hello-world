<template>
  <div class="p-8">
    <h1 class="text-4xl font-bold">{{ filmData?.title }}</h1>

    <ul class="mt-4 list-disc pl-8">
      <li
        v-for="character in filmData?.charactersInfo"
        :key="character.name"
        class="text-xl">
        <a class="text-blue-500 underline">{{ character.name }}</a>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';

const props = defineProps({
  episodeId: {
    type: String,
    required: true,
  },
});

const filmData = ref(null);

onMounted(async () => {
  const response = await axios.get(`https://swapi.dev/api/films/${props.episodeId}`);
  filmData.value = response.data;
  filmData.value.charactersInfo = [];

  filmData.value.characters.forEach(async (character) => {
    const response = await axios.get(character);
    filmData.value.charactersInfo.push(response.data);
  });
});
</script>
