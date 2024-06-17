<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { srcPngHelper } from '@/components/utils';

import IsError from '../IsError.vue';
import ImgItem from './ImgItem.vue';

const namesImages = ref([]);
const errorState = ref('');

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:3000/images');
    namesImages.value = response.data.map((fileName) => fileName.replace('.png', ''));
  } catch (error) {
    errorState.value = `Error fetching PNG files: ${error}`;
  }
});
</script>

<template>
  <div class="change-imgs">
    <ul>
      <ImgItem v-for="name in namesImages" :key="name" :name="name" :src="srcPngHelper(name)" />
    </ul>
    <IsError :error-state="errorState" />
  </div>
</template>

<style scoped>
.change-imgs {
  height: 55vh;
  overflow-y: auto;
}

ul {
  flex-direction: row;
  flex-wrap: wrap;
}

.game__about.active {
  position: absolute;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 30px;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}
</style>
