<script setup>
import { inject } from 'vue';

const data = inject('dataAdmin');
</script>

<template>
  <div class="change-users">
    <ul v-if="data.users !== undefined">
      <li>
        <h2 v-for="key in Object.keys(data.users[0])" :key="key">{{ key }}</h2>
      </li>
      <li v-for="user in data.users" :key="user.name">
        <input type="text" v-model="user.username" placeholder="username" />
        <input type="text" v-model="user.password" placeholder="password" />
        <input type="email" v-model="user.email" placeholder="email" />
      </li>
    </ul>
    <h1 v-else class="warning">Пустой</h1>
  </div>
</template>
