<script setup>
import { inject, ref, toRaw, computed, provide } from 'vue';
import { srcPngHelper } from '@/components/utils';

import SelectedGameItem from './selectedGameItem.vue';

const data = inject('data');

const popupActive = ref(false);
const isScrollBlocked = ref(false);
const totalPrice = ref(0);

provide('totalPrice', totalPrice);

const toggleScroll = () => {
  if (isScrollBlocked.value) {
    document.body.style.overflow = '';
  } else {
    document.body.style.overflow = 'hidden';
  }
  isScrollBlocked.value = !isScrollBlocked.value;
};

const openPopup = () => {
  popupActive.value = true;
  toggleScroll();
};

const closePopup = () => {
  popupActive.value = false;
  toggleScroll();
};

const clearPopup = () => {
  closePopup();
  data.selectGames.value = [];
};

const filteredGames = computed(() => {
  const selectedKeys = Object.values(data.selectGames.value).map(Number);
  const parsedGames = toRaw(data.games.value).value;
  return parsedGames.filter((game) => selectedKeys.includes(game.id));
});

const removeGame = (removeId) => {
  const updatedArr = data.selectGames.value.filter((selectId) => selectId != removeId);
  data.selectGames.value = updatedArr;
};
</script>

<template>
  <button @click="openPopup">
    <i class="counter" v-if="filteredGames.length">{{ filteredGames.length }}</i>
    {{ data.user.value.name }}
  </button>
  <Teleport to="body">
    <div v-show="popupActive" class="popupAccount-bg" @click="closePopup">
      <div class="popupAccount" @click.stop>
        <h1>Выбранные игры для покупки</h1>
        <SelectedGameItem
          v-for="game in filteredGames"
          :key="game.id"
          :id="game.id"
          :name="game.nameGame"
          :img-src="srcPngHelper(game.imgSrc)"
          :price="game.money"
          @remove="removeGame"
        />
        <h2 v-show="filteredGames.length">
          Итого:
          <span> {{ totalPrice }} </span>
        </h2>
        <button @click="clearPopup">очистить корзину</button>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
button {
  color: #1e3072;
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 30px;
}

.popupAccount button {
  position: absolute;
  right: 20px;
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

i {
  font-size: 15px;
  text-align: center;
  border-radius: 50%;
  padding: 2px 8px;
  background-color: coral;
}

.popupAccount {
  position: relative;
  height: 80%;
  width: 40%;
  display: flex;
  flex-direction: column;
  align-self: center;
  background-color: rgb(255, 255, 255);
  border-radius: 2%;
  opacity: 1;
  padding: 30px;
  gap: 30px;
}

.popupAccount > h2 {
  text-align: right;
}

.popupAccount__check {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px;
}
</style>
