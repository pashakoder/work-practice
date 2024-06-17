<template>
  <RouterLink class="logo" to="/">
    <img src="/GamerMarket.svg" alt="Logo" @dragstart.prevent />
    <h1 class="logo__text" @dragstart.prevent>GamerMarket</h1>
  </RouterLink>
</template>

<style scoped>
.logo {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 5px;
}

.logo__text {
  font-style: normal;
  font-weight: 700;
  font-size: 24px;
  line-height: 28px;
  color: #1e3072;
}
</style>
