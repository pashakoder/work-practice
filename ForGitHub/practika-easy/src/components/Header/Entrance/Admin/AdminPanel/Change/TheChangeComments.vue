<script setup>
import { inject } from 'vue';

const data = inject('dataAdmin');
</script>

<template>
  <div class="change-comments">
    <ul v-if="data.comments  !== undefined">
      <li>
        <h2 v-for="key in Object.keys(data.comments[0])" :key="key">{{ key }}</h2>
      </li>
      <li v-for="(comment, index) in data.comments" :key="index">
        <input type="text" v-model="comment.username" placeholder="username" />
        <input type="text" v-model="comment.mail" placeholder="email" />
        <input type="email" v-model="comment.text" placeholder="text" />
      </li>
    </ul>
    <h1 v-else class="warning">Пустой</h1>
  </div>
</template>
