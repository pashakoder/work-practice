<script setup>
import { inject } from 'vue';

const data = inject('dataAdmin');
</script>

<template>
  <div class="change-games">
    <ul v-if="data.games !== undefined">
      <li class="row-game">
        <h2 v-for="key in Object.keys(data.games[0])" :key="key">{{ key }}</h2>
      </li>
      <li v-for="game in data.games" :key="game.id" class="row-game">
        <input type="text" v-model="game.nameGame" placeholder="Название игры" />
        <div class="labels">
          <label v-for="platform in ['All', 'PC', 'PlayStation', 'Nintendo']" :key="platform">
            <input type="checkbox" v-model="game.platforms" :value="platform" />
            {{ platform }}
          </label>
        </div>
        <input type="text" v-model="game.imgSrc" placeholder="imgSrc" />
        <input type="number" min="0" v-model="game.money" placeholder="Цена" />
        <input type="number" v-model="game.id" placeholder="Id" />
      </li>
    </ul>
    <h1 v-else class="warning">Пустой</h1>
  </div>
</template>

<style scoped>
.change-games {
  width: 100%;
}

.row-game h2:nth-child(2) {
  width: 150px;
}

.labels {
  display: flex;
  flex-direction: column;
  width: 150px;
}

.labels input {
  width: 10px;
}

input {
  width: 100%;
}

input:last-child {
  width: 30px;
}
</style>
