<script setup>
import { ref, inject } from 'vue';
import axios from 'axios';

import IsSuccessful from '../IsSuccessful.vue';
import IsError from '../IsError.vue';

const data = inject('data');

const errorPost = ref('');
const success = ref('');
const file = ref(null);
const fileName = ref('');

const onFileChange = (event) => {
  file.value = event.target.files[0];
};

const uploadFile = async () => {
  if (!file.value) {
    alert('Please select a file first!');
    return;
  }

  const formData = new FormData();
  formData.append('file', file.value, fileName.value);
  console.log(data.user.value.name);
  formData.append('username', data.user.value.name);
  formData.append('password', data.user.value.passAdmin);

  try {
    const response = await axios.post('http://localhost:3000/upload', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    });
    success.value = response.data;
    errorPost.value = '';
  } catch (error) {
    errorPost.value = error;
  }
};
</script>

<template>
  <div class="change-imgs">
    <h3>Фото загружается сразу, не нужно сохранять изменения</h3>
    <input type="file" @change="onFileChange" />
    <input type="text" v-model="fileName" placeholder="Enter new file name" />
    <IsError :errorState="errorPost" />
    <button @click="uploadFile">Загрузить на сервер</button>
    <IsSuccessful :successState="success" />
  </div>
</template>

<style scoped>
.change-imgs {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.change-imgs button {
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
