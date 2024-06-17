<script setup>
import { ref, watch, inject, onMounted, onUnmounted } from 'vue';

const total = inject('totalPrice');

const props = defineProps(['id', 'name', 'price', 'imgSrc']);
defineEmits(['remove']);

const count = ref(1);

watch(count, (newCount, oldCount) => {
  if (newCount >= oldCount) {
    total.value += (newCount - oldCount) * props.price;
    return;
  }
  total.value -= (oldCount - newCount) * props.price;
});

onMounted(() => {
  total.value += props.price;
});

onUnmounted(() => {
  total.value -= count.value * props.price;
});
</script>

<template>
  <div class="game">
    <div class="unite">
      <input type="number" min="0" v-model="count" />
      <img :src="imgSrc" :alt="name" />
    </div>
    <h2>{{ name }}</h2>
    <p>{{ price }}</p>
    <button @click="$emit('remove', id)">❌</button>
  </div>
</template>

<style scoped>
.unite {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.unite input {
  width: 40px;
}

.game {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: min-content;
}

.game img {
  height: 130px;
  border-radius: 25px;
}
</style>
