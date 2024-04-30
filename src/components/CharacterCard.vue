<template>
  <div
    :key="person?.name"
    class="border border-gray-300 p-8">
    <h2 class="text-2xl font-bold">{{ person?.name }}</h2>

    <ul class="mt-4 list-disc pl-8">
      <li
        v-for="film in person?.filmsInfo"
        :key="film?.title"
        class="text-xl">
        <a
          :href="`/film/${film?.episode_id}`"
          class="text-blue-500 underline"
          >{{ film?.title }}</a
        >
      </li>
    </ul>
  </div>
</template>

<script setup>
const props = defineProps({
  person: {
    type: Object,
    required: true,
  },
});
</script>
