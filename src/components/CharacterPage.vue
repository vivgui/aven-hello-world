<template>
  <div class="p-8">
    <div v-if="isLoading">Loading Character Info...</div>
    <div v-else>
      <CharacterCard :person="person" />

      <GoBack />
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import CharacterCard from './CharacterCard.vue';
import GoBack from './GoBack.vue';

const props = defineProps({
  characterId: {
    type: String,
    required: true,
  },
});
const personAPI = `https://swapi.dev/api/people/${props.characterId}`;

const person = ref(null);
const isLoading = ref(true);

onMounted(async () => {
  isLoading.value = true;
  const response = await axios.get(personAPI);
  person.value = response.data;

  await Promise.all(
    person.value.films.map((film) => {
      const filmPromises = person.value.films.map((film) => axios.get(film));
      return Promise.all(filmPromises).then((filmResponses) => {
        person.value.filmsInfo = filmResponses.map((filmResponse) => filmResponse.data);
      });
    }),
  );

  isLoading.value = false;
});
</script>
