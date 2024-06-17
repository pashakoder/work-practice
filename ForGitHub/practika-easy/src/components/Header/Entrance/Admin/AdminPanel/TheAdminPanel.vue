<script setup>
import axios from 'axios';
import { inject, onMounted, reactive, watch, ref } from 'vue';

import AddUser from './Add/TheAddUser.vue';
import AddNew from './Add/TheAddNew.vue';
import AddComment from './Add/TheAddComment.vue';
import AddGame from './Add/TheAddGame.vue';
import UploadImg from './Add/TheUploadImg.vue';

import ChangeUsers from './Change/TheChangeUsers.vue';
import ChangeNews from './Change/TheChangeNews.vue';
import ChangeComments from './Change/TheChangeComments.vue';
import ChangeGames from './Change/TheChangeGames.vue';
import ChangeImgs from './Change/TheChangeImgs.vue';

import IsError from './IsError.vue';

const data = inject('dataAdmin');
const dataUser = inject('data');
const section = inject('section');
const subSection = inject('subSection');
const updateDataPressed = inject('updateDataPressed');

const errorState = ref('');

const cleanArray = (array) => {
  return array.filter((obj) => !Object.values(obj).some((value) => value === '' || value === null));
};

const updateData = async () => {
  const user = dataUser.user.value;
  for (let key in data) {
    if (Array.isArray(data[key])) {
      data[key] = cleanArray(data[key]);
    }
  }
  console.log(data);
  try {
    await axios.post('http://localhost:3000/admin_update', {
      username: user.name,
      password: user.passAdmin,
      data
    });
  } catch (error) {
    errorState.value = error;
  }
  location.reload();
};

onMounted(async () => {
  if (!Object.keys(data).length) {
    const user = dataUser.user.value;
    try {
      const response = await axios.post('http://localhost:3000/admin_data', {
        username: user.name,
        password: user.passAdmin
      });
      ['users', 'news', 'games', 'comments'].forEach(
        (el) => (data[el] = reactive(response.data[el]))
      );
    } catch (error) {
      console.error(error);
    }
  }
  watch(updateDataPressed, async (pressed) => {
    if (pressed) {
      updateData();
      updateDataPressed.value = false; // сброс
    }
  });
});
</script>

<template>
  <div v-if="section == 'Добавить'" class="add">
    <AddUser v-if="section == 'Добавить' && subSection == 'Users'" />
    <AddNew v-if="section == 'Добавить' && subSection == 'News'" />
    <AddComment v-if="section == 'Добавить' && subSection == 'Comments'" />
    <AddGame v-if="section == 'Добавить' && subSection == 'Games'" />
    <UploadImg v-if="section == 'Добавить' && subSection == 'Img'" />
  </div>
  <div v-if="section == 'Изменить'" class="change">
    <h1 v-if="subSection != 'Img'" class="warning">Изменения происходят реактивно!</h1>
    <h1 v-else class="warning">Изменения происходят сразу на сервере!</h1>
    <ChangeUsers v-if="section == 'Изменить' && subSection == 'Users'" />
    <ChangeNews v-if="section == 'Изменить' && subSection == 'News'" />
    <ChangeComments v-if="section == 'Изменить' && subSection == 'Comments'" />
    <ChangeGames v-if="section == 'Изменить' && subSection == 'Games'" />
    <ChangeImgs v-if="section == 'Изменить' && subSection == 'Img'" />
  </div>
  <IsError :error-state="errorState" />
</template>

<style scoped>
.change {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
