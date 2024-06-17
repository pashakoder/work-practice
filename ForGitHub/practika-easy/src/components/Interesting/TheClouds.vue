<template>
  <div class="clouds">
    <img
      v-for="(cloud, index) in clouds"
      :key="index"
      :src="cloud.src"
      :class="cloud.class"
      :style="cloud.style"
      :alt="cloud.alt"
    />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const clouds = ref([
      { src: '/cloud.png', alt: 'Cloud 1', class: 'cloud' },
      { src: '/cloud2.png', alt: 'Cloud 2', class: 'cloud' },
      { src: '/cloud.png', alt: 'Cloud 3', class: 'cloud' },
      { src: '/cloud2.png', alt: 'Cloud 4', class: 'cloud' }
    ]);

    onMounted(() => {
      clouds.value.forEach((cloud, index) => {
        const step = 20; // Шаг в процентах
        const randomTop = Math.random() * (100 - step) + index * step; // Случайное значение с шагом
        const randomDuration = Math.random() * 30 + 20; // Случайное значение от 20s до 40s
        const randomDelay = Math.random() * 10; // Случайное значение задержки от 0s до 10s
        cloud.style = {
          top: `${randomTop}%`,
          animationDuration: `${randomDuration}s`,
          animationDelay: `${randomDelay}s`,
          left: '-20%'
        };
      });
    });

    return {
      clouds
    };
  }
};
</script>

<style scoped>
.clouds {
  z-index: -2;
  position: absolute;
  height: 100%;
  width: 100%;
  overflow: hidden;
  margin-left: -10px;
  top: 50px;
}

.cloud {
  position: absolute;
  animation: moveClouds linear infinite;
  z-index: -2;
}

@keyframes moveClouds {
  0% {
    left: -20%;
  }
  100% {
    left: 120%;
  }
}
</style>
