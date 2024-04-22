<template>
  <div class="p-8 pt-16">
    <div class="flex flex-col items-center justify-center">
      <h1 class="text-4xl font-bold text-indigo-500 underline">
        <a
          :href="storyData?.url"
          target="_blank"
          >{{ storyData?.title }}</a
        >
      </h1>
      <div class="mt-4 flex items-center gap-4">
        <a>{{ storyData?.by }}</a>
        <span>{{ new Date(storyData?.time * 1000).toLocaleString() }}</span>
        <a
          :href="storyData?.url"
          class="underline"
          >{{ storyData?.url.split('/')[2] }}</a
        >
      </div>
    </div>
    <ul class="mt-16 list-none space-y-8 divide-y-2 divide-gray-300 border-2">
      <li
        v-for="comment in comments"
        class="px-8 pt-8 text-gray-600">
        <div class="mb-4 flex items-center gap-4">
          <p class="text-sm">{{ comment.by }}</p>
          <p class="text-sm">{{ new Date(comment.time * 1000).toLocaleString() }}</p>
        </div>
        <p
          class="text-2xl text-gray-700"
          v-html="comment.text"></p>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const props = defineProps({
  storyId: {
    type: String,
    required: true,
  },
});

const comments = ref([]);
const storyData = ref(null);

const itemUrl = `https://hacker-news.firebaseio.com/v0/item/${props.storyId}.json`;
const commentUrl = `https://hacker-news.firebaseio.com/v0/item/`;

axios.get(itemUrl).then((response) => {
  storyData.value = response.data;
});

axios.get(itemUrl).then((response) => {
  response.data?.kids.forEach((comment) => {
    axios.get(commentUrl + comment + '.json').then((response) => {
      comments.value.push(response.data);
    });
  });
});
</script>
