<script setup>
defineProps(['errorState']);
</script>

<template>
  <p v-if="errorState != ''" class="error">{{ errorState }}</p>
</template>

<style scoped>
.error {
  color: red;
}
</style>
