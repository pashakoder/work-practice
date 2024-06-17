<script setup>
import { inject, reactive, ref, toRaw } from 'vue';
import IsSuccessful from '../IsSuccessful.vue';
import IsError from '../IsError.vue';

const newUser = reactive({
  username: '',
  password: '',
  email: ''
});

const error = ref('');

const success = ref(false);

const data = inject('dataAdmin');

const validate = () => {
  let valid = true;

  if (toRaw(data.users).some((user) => user.username === newUser.username)) {
    error.value = 'Имя уже занято';
    valid = false;
  }

  return valid;
};

const addNewUser = () => {
  if (!validate()) {
    success.value = false;
    return;
  }
  success.value = true;
  data.users.push({ ...newUser });
  reset();
};

const reset = () => {
  newUser.email = '';
  newUser.username = '';
  newUser.password = '';
  error.value = '';
};
</script>

<template>
  <div class="change-users">
    <input type="text" v-model="newUser.username" placeholder="Имя" />
    <input type="text" v-model="newUser.password" placeholder="Пароль" />
    <input type="email" v-model="newUser.email" placeholder="Почта" />
    <IsError :errorState="error" />
    <button @click="addNewUser">добавить в изменения</button>
    <IsSuccessful :successState="success" />
  </div>
</template>

<style scoped>
.change-users {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.change-users button {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  width: fit-content;
  background: #9bf00b;
  font-weight: 700;
  font-size: 16px;
  line-height: 19px;
  color: #054b16;
}
</style>
