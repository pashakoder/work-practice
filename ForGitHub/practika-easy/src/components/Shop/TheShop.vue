<script setup>
import { provide, ref, inject, watch, toRaw } from 'vue';

import Title from './TheTitle.vue';
import ShopItem from './ShopItem.vue';

const selected = ref('All');

provide('selectedPlatforms', selected);

const data = inject('data');
const apply = inject('apply');
const gamesArr = ref([]);

watch(apply, () => {
  const news = toRaw(data.games.value).value;
  gamesArr.value = news;
});
</script>

<template>
  <div class="shop">
    <Title :selected-section="selected" @select-section="(s) => (selected = s)" />
    <div class="shop__body">
      <ShopItem
        v-for="{ nameGame, platforms, imgSrc, money, id } in gamesArr"
        :key="id"
        :id="id"
        :name-game="nameGame"
        :platforms="platforms"
        :img-src="imgSrc"
        :money="money"
      />
    </div>
    <img src="/castle.png" alt="Пиксельный замок" />
  </div>
</template>

<style scoped>
.shop {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 0 10px;
  background-color: #6b8cff;
}

.shop__body {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-start;
  align-content: flex-start;
  padding: 10px;
  gap: 50px 80px;
}

.shop > img {
  margin: 0 -10px;
}
</style>
