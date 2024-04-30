<template>
  <div class="grid grid-cols-3 gap-8 p-8">
    <div
      v-for="person in people"
      :key="person.name"
      class="border border-gray-300 p-8">
      <h2 class="text-2xl font-bold">{{ person.name }}</h2>

      <ul class="mt-4 list-disc pl-8">
        <li
          v-for="film in person.filmsInfo"
          :key="film.title"
          class="text-xl">
          <a
            :href="`/film/${film.episode_id}`"
            class="text-blue-500 underline"
            >{{ film.title }}</a
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';

const peopleAPI = 'https://swapi.dev/api/people/';
const people = ref(null);

onMounted(async () => {
  const response = await axios.get(peopleAPI);

  people.value = response.data.results;

  people.value.forEach((person) => {
    person.filmsInfo = [];
    person.films.forEach(async (film) => {
      const response = await axios.get(film);
      person.filmsInfo.push(response.data);
    });
  });
});
</script>
