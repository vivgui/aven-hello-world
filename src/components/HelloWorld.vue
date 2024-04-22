<template>
  <div class="p-8">
    <h1 class="text-2xl font-bold">{{ message }}</h1>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const message = ref('Hello, World!');
</script>
