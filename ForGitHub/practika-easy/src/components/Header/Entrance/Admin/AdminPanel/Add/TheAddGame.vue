<script setup>
import { inject, reactive, ref, toRaw, onMounted } from 'vue';
import IsSuccessful from '../IsSuccessful.vue';
import IsError from '../IsError.vue';

const newGame = reactive({
  nameGame: '',
  platforms: ['All'],
  imgSrc: '',
  money: '',
  id: ''
});

const error = ref('');
const success = ref(false);

const data = inject('dataAdmin');

const validate = () => {
  let valid = true; // Change

  if (newGame.money == '') {
    error.value = 'Не указана цена';
    valid = false;
  }
  return valid;
};

const addNewGame = () => {
  if (!validate()) {
    success.value = false;
    return;
  }
  success.value = true;
  data.games.push({ ...newGame });
  reset();
};

const reset = () => {
  newGame.nameGame = '';
  newGame.platforms = ['All'];
  newGame.imgSrc = '';
  newGame.money = 0;
  newGame.id = 0;
};

const missingId = () => {
  const sortedIds = data.games.map((game) => game.id).sort((a, b) => a - b);

  for (let i = 0; i < sortedIds.length; i++) {
    if (sortedIds[i + 1] !== undefined && sortedIds[i + 1] !== sortedIds[i] + 1) {
      return sortedIds[i] + 1;
    }
  }

  return sortedIds[sortedIds.length - 1] + 1;
};

onMounted(() => {
  const lastId = toRaw(data.games).at(-1).id;
  const missId = missingId();
  if (!missId == lastId) {
    newGame.id = missId;
    return;
  }
  newGame.id = lastId + 1;
});
</script>

<template>
  <div class="change-games">
    <input type="text" v-model="newGame.nameGame" placeholder="Название игры" />
    <div class="platforms">
      <h3>Platforms:</h3>
      <label v-for="platform in ['All', 'PC', 'PlayStation', 'Nintendo']" :key="platform">
        <input type="checkbox" v-model="newGame.platforms" :value="platform" />
        {{ platform }}
      </label>
    </div>
    <input type="text" v-model="newGame.imgSrc" placeholder="imgSrc" />
    <input type="number" min="0" v-model="newGame.money" placeholder="Цена" />
    <input type="number" v-model="newGame.id" placeholder="Id" />
    <IsError :errorState="error" />
    <button @click="addNewGame">добавить в изменения</button>
    <IsSuccessful :successState="success" />
  </div>
</template>

<style scoped>
.change-games {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.platforms {
  display: flex;
  gap: 30px;
}

.change-games button {
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
