<template>
  <div class="p-8">
    <div v-if="isLoading">Loading Film Info...</div>
    <div v-else>
      <h1 class="text-4xl font-bold">{{ filmData?.title }}</h1>
      <ul class="mt-4 list-disc pl-8">
        <li
          v-for="character in filmData?.charactersInfo"
          :key="character.name"
          class="text-xl">
          <a
            class="text-blue-500 underline"
            :href="`/character/${character.url.split('/').at(-2)}`"
            >{{ character.name }}</a
          >
        </li>
      </ul>

      <GoBack />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import GoBack from './GoBack.vue';

const props = defineProps({
  episodeId: {
    type: String,
    required: true,
  },
});

const filmData = ref(null);
const isLoading = ref(true);

onMounted(async () => {
  isLoading.value = true;
  const response = await axios.get(`https://swapi.dev/api/films/${props.episodeId}`);
  filmData.value = response.data;
  filmData.value.charactersInfo = [];

  const characterPromises = filmData.value.characters.map((characterUrl) => axios.get(characterUrl));

  const characterResponses = await Promise.all(characterPromises);
  filmData.value.charactersInfo = characterResponses.map((res) => res.data);

  isLoading.value = false;
});
</script>
