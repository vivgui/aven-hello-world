<template>
  <div class="grid grid-cols-3 gap-8 p-8">
    <CharacterCard
      v-for="person in people"
      :key="person.name"
      :person="person" />
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import CharacterCard from './CharacterCard.vue';

const peopleAPI = 'https://swapi.dev/api/people/';
const people = ref([]);

async function fetchPeople(url) {
  const response = await axios.get(url);
  people.value = [...people.value, ...response.data.results];

  if (response.data.next) {
    await fetchPeople(response.data.next);
  }
}

onMounted(async () => {
  await fetchPeople(peopleAPI);

  await Promise.all(
    people.value.map((person) => {
      const filmPromises = person.films.map((film) => axios.get(film));
      return Promise.all(filmPromises).then((filmResponses) => {
        person.filmsInfo = filmResponses.map((filmResponse) => filmResponse.data);
      });
    }),
  );
});
</script>
