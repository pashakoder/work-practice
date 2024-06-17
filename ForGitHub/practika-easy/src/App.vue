<script setup>
import { RouterView } from 'vue-router';
import { provide, reactive, toRefs, onMounted, ref } from 'vue';
import axios from 'axios';

import Header from '@/components/Header/TheHeader.vue';

const apply = ref(false);

const data = reactive({
  user: {
    name: '', // ''
    isLogged: false, // false
    isAdmin: false, // false
    passAdmin: '' // ''
  },
  news: [],
  games: [],
  selectGames: []
});

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:3000/data');
    data.news.value = response.data.news;
    data.games.value = response.data.games;
    apply.value = true;
  } catch (error) {
    console.error('Get data failed:', error);
  }
});

provide('data', toRefs(data));
provide('apply', apply);
</script>

<template>
  <Header />

  <RouterView />
</template>

<style scoped></style>
