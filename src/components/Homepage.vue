<template>
  <div class="p-8">
    <ul class="list-decimal space-y-8 divide-y divide-gray-300 pl-8">
      <li
        v-for="story in news"
        class="pt-8 marker:text-3xl marker:text-indigo-500">
        <div class="flex justify-between">
          <a
            :href="`/${story.id}`"
            class="text-3xl text-indigo-500 hover:text-indigo-700"
            >{{ story.title }}</a
          >
          <span class="flex flex-col text-right">
            <span class="text-3xl text-gray-600">{{ story.score }}p</span>
          </span>
        </div>
        <div>
          <span>({{ story.url.split('/')[2] }})</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const hnurl = 'https://hacker-news.firebaseio.com/v0/topstories.json';
const itemurl = 'https://hacker-news.firebaseio.com/v0/item/';

const news = ref([]);

axios.get(hnurl).then((response) => {
  const topStoriesIds = response.data.slice(0, 10);
  topStoriesIds.forEach((story) => {
    axios.get(itemurl + story + '.json').then((response) => {
      news.value.push(response.data);
    });
  });
});
</script>
