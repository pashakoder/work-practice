<script setup>
import { inject, reactive, ref } from 'vue';
import IsSuccessful from '../IsSuccessful.vue';
import IsError from '../IsError.vue';

const newComment = reactive({
  name: '',
  email: '',
  text: ''
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
  data.comments.push({ ...newComment });
  reset();
};

const reset = () => {
  newComment.name = '';
  newComment.email = '';
  newComment.text = '';
  error.value = '';
};
</script>

<template>
  <div class="add-comment">
    <input type="text" v-model="newComment.name" placeholder="name" />
    <input type="text" v-model="newComment.email" placeholder="email" />
    <input type="text" v-model="newComment.text" placeholder="text" />
    <IsError :errorState="error" />
    <button @click="addNewNew">добавить в изменения</button>
    <IsSuccessful :successState="success" />
  </div>
</template>

<style scoped>
.add-comment {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.add-comment button {
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
