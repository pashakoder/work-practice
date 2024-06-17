<script setup>
import { inject } from 'vue';

const data = inject('dataAdmin');
</script>

<template>
  <div class="change-news">
    <ul v-if="data.news !== undefined">
      <li>
        <h2 v-for="key in Object.keys(data.news[0])" :key="key">{{ key }}</h2>
      </li>
      <li v-for="(newData, index) in data.news" :key="index">
        <input type="text" v-model="newData.title" placeholder="title" />
        <input type="text" v-model="newData.author" placeholder="author" />
        <input type="email" v-model="newData.imgSrc" placeholder="imgSrc" />
      </li>
    </ul>
    <h1 v-else class="warning">Пустой</h1>
  </div>
</template>
