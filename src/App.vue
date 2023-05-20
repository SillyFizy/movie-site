<template>
  <div>
    <NavBar>
      <input type="text" placeholder="Search for a movie" class="text-black mr-5 p-2 rounded-3xl outline-none"
        v-model="query" @keyup.enter="getResults(query)">
    </NavBar>
    <div class="flex w-full justify-center">
      <div class="w-3/4">
        <h1 class="font-bold text-2xl mb-4">Trending Movies</h1>
        <div class="grid grid-cols-4 gap-4">
          <Search v-for="result in searchResults" :result="result"></Search>
          <Movie v-for="movie in trendingMoviesDay" :movie="movie" class=""></Movie>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

import axios from 'axios';
import { onMounted, ref, computed } from 'vue';
import NavBar from './components/NavBar.vue';
import Movie from './components/Movie.vue';
import Search from './components/Search.vue'

const trendingMoviesDay = ref([]);
const query = ref([]);
const searchResults = ref([]);

const fetchTrendingMoviesDay = async () => {
  const response = await axios.get('https://api.themoviedb.org/3/trending/movie/day', {
    headers: {
      Authorization: "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJlOGJmZmU5NzZmZGFkZGY1MWFkNjA5MDA5MDc1ZDhlMyIsInN1YiI6IjY0NjY0NmIwMmJjZjY3MDE3MmIwMGJiYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.s1KODC5ki2ejnvl8lvlemhCB6LXetJmtYjQ1FD1T-fY"
    }
  })
  trendingMoviesDay.value = response.data.results;
}
const getResults = async (query) => {
  if (query.value == '') {
    return
  }
  const response = await axios.get('https://api.themoviedb.org/3/search/movie?query=' + query + '&include_adult=false&language=en-US', {
    headers: {
      Authorization: "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJlOGJmZmU5NzZmZGFkZGY1MWFkNjA5MDA5MDc1ZDhlMyIsInN1YiI6IjY0NjY0NmIwMmJjZjY3MDE3MmIwMGJiYSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.s1KODC5ki2ejnvl8lvlemhCB6LXetJmtYjQ1FD1T-fY"
    }
  })
  searchResults.value = response.data.results;
  query.value = '';
}
onMounted(() => {
  fetchTrendingMoviesDay();
})
</script>
