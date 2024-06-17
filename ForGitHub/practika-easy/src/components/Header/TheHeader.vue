<script setup>
import Logo from './TheLogo.vue';
import Entrance from './Entrance/TheEntrance.vue';
</script>

<template>
  <header class="header">
    <Logo />
    <Entrance />
  </header>
</template>

<style scoped>
.header {
  z-index: 10;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0px 10px;
  gap: 10px;
  height: 50px;
  background: #3f57ae;
}

.header__search {
  width: 200px;
  background-color: #293d86;
}
</style>
