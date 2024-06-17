<script setup>
import { inject } from 'vue';

import Login from './Guest/TheLogin.vue';
import Registration from './Guest/TheRegistration.vue';
import Account from './User/TheAccount.vue';
import Admin from './Admin/TheAdmin.vue';

const data = inject('data');
</script>

<template>
  <div v-if="!data.user.value.isLogged" class="color guest">
    <Login />
    <Registration />
  </div>
  <div v-else-if="data.user.value.isAdmin" class="color-admin">
    <Admin />
  </div>
  <div v-else class="color account">
    <Account />
  </div>
</template>

<style scoped>
.color {
  color: #1e3072;
  display: flex;
  gap: 10px;
}

.color-admin {
  color: red;
}

button {
  color: inherit;
}
</style>
