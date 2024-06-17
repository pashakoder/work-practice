<script setup>
import axios from 'axios';
import { inject, ref } from 'vue';
const data = inject('data');

const props = defineProps(['name', 'src']);

const isActive = ref(false);

const remove = () => {
  axios.post('http://localhost:3000/removeImg', {
    username: data.user.value.name,
    password: data.user.value.passAdmin,
    pngDel: props.name
  });
};
</script>

<template>
  <div class="img-item">
    <img :src="src" @mouseover="isActive = !isActive" />
    <div :class="['about', { active: isActive }]" @mouseleave="isActive = !isActive">
      <h1>{{ name }}</h1>
      <h2>
        <a :href="src">{{ src }}</a>
      </h2>
      <button @click="remove">❌</button>
    </div>
  </div>
</template>

<style scoped>
.img-item {
  display: flex;
  width: 230px;
  height: 200px;
  position: relative;
}

.img-item > img {
  cursor: pointer;
  height: 100%;
  width: 100%;
}

.about {
  display: none;
}

.about h1 {
  color: #fff;
}

.about h2 {
  font-size: 10px;
  color: #fff;
}

.about.active {
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
