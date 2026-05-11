<script setup lang="ts">
import axios from 'axios';
import { ref } from 'vue';

const movies = ref<any[]>([])

axios.get('https://movie.pequla.com/api/movie').then(rsp => movies.value = rsp.data);
</script>

<template>
  <p>Movies: </p>
  <ul>
    <li v-for="movie in movies">
      {{ movie.title }}
    </li>
  </ul>
</template>

<style scoped></style>
