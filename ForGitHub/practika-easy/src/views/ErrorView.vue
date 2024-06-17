<script setup>
import { useRouter } from 'vue-router';
const router = useRouter();

function reloadPage() {
  router.push('/').then(() => {
    window.location.reload();
  });
}
</script>

<template>
  <div class="er404">
    <h1>ERROR 404</h1>
    <RouterLink class="logo" to="/" @click.prevent="reloadPage"> Вернуться обратно! </RouterLink>
  </div>
</template>

<style scoped>
.er404 {
  height: calc(100vh - 50px);
  background-color: #3f57ae;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 20px;
}

.er404 h1 {
  font-size: 98px;
  color: red;
}
</style>
