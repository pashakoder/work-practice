<script setup>
defineProps(['successState']);
</script>

<template>
  <div v-if="typeof successState != 'boolean'" class="success">
    <p>{{ successState }}</p>
  </div>
  <div v-else-if="successState" class="success">
    <p>successful, не забудьте сохранить изменения</p>
  </div>
</template>

<style scoped>
.success {
  width: 100%;
  display: flex;
  justify-content: center;
  color: #054b16;
}
</style>
