<script setup>
import { inject, reactive, ref } from 'vue';
import IsSuccessful from '../IsSuccessful.vue';
import IsError from '../IsError.vue';

const newNew = reactive({
  title: '',
  author: '',
  imgSrc: ''
});

const error = ref('');
const success = ref(false);

const data = inject('dataAdmin');

const validate = () => {
  let valid = true; // Change

  return valid;
};

const addNewNew = () => {
  if (!validate()) {
    success.value = false;
    return;
  }
  success.value = true;
  data.news.push({ ...newNew });
  reset();
};

const reset = () => {
  newNew.title = '';
  newNew.author = '';
  newNew.imgSrc = '';
  error.value = '';
};
</script>

<template>
  <div class="change-news">
    <input type="text" v-model="newNew.title" placeholder="Title" />
    <input type="text" v-model="newNew.author" placeholder="Author" />
    <input type="email" v-model="newNew.imgSrc" placeholder="ImgSrc" />
    <IsError :errorState="error" />
    <button @click="addNewNew">добавить в изменения</button>
    <IsSuccessful :successState="success" />
  </div>
</template>

<style scoped>
.change-news {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.change-news button {
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
