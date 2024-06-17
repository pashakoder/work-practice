<script setup>
import { inject, toRaw, ref, watch } from 'vue';
import { srcPngHelper } from '@/components/utils';

import NewItem from './NewItem.vue';
import TheClouds from './TheClouds.vue';

const data = inject('data');
const apply = inject('apply');
const newsArr = ref([]);

watch(apply, () => {
  const news = toRaw(data.news.value).value;
  newsArr.value = news;
});
</script>

<template>
  <div class="interesting">
    <div class="interesting__news">
      <h1 class="interesting__news-title">Новости в ГеймДеве</h1>
      <div class="interesting__news-body">
        <NewItem
          v-for="{ title, author, imgSrc } in newsArr"
          :key="imgSrc"
          :title="title"
          :author="author"
          :img-src="srcPngHelper(imgSrc)"
        />
        <TheClouds />
      </div>
    </div>
  </div>
</template>

<style scoped>
.interesting {
  display: flex;
  padding: 35px 10px 30px;
  gap: 20px;
  isolation: isolate;
  background: linear-gradient(180deg, #3f57ae 0%, #6b8cff 100%);
}

.interesting__news {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.interesting__news-title {
  display: flex;
  justify-content: start;
  align-items: center;
  gap: 10px;
  width: 100%;
  font-style: normal;
  font-weight: 400;
  font-size: 64px;
  line-height: 75px;
  color: #1e3072;
}

.interesting__news-body {
  z-index: -3;
  display: flex;
  justify-content: center;
  gap: 20px;
}

.interesting__news-body {
  flex-direction: column;
}

.interesting__carts-body {
  justify-content: start;
  gap: 30px;
}
</style>
