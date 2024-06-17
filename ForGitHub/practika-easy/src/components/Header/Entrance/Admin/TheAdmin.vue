<script setup>
import { inject, provide, ref } from 'vue';
import ThePopupMain from './ThePopupMain.vue';

const data = inject('data');

const popupActive = ref(false);
const isScrollBlocked = ref(false);
const updateDataPressed = ref(false);

const toggleScroll = () => {
  if (isScrollBlocked.value) {
    document.body.style.overflow = '';
  } else {
    document.body.style.overflow = 'hidden';
  }
  isScrollBlocked.value = !isScrollBlocked.value;
};

const openPopup = () => {
  popupActive.value = true;
  toggleScroll();
};

const closePopup = () => {
  popupActive.value = false;
  toggleScroll();
};

const updateData = () => {
  updateDataPressed.value = true;
};

provide('updateDataPressed', updateDataPressed);
</script>

<template>
  <button @click="openPopup">{{ data.user.value.name }}</button>
  <Teleport to="body">
    <div v-show="popupActive" class="popupAdmin-bg" @click="closePopup">
      <div class="popupAdmin" @click.stop>
        <h1>Настройки</h1>
        <ThePopupMain />
        <button @click="updateData">сохранить изменения</button>
      </div>
    </div>
  </Teleport>
</template>

<style scoped>
button {
  color: red;
  gap: 5px;
  font-size: 30px;
}

.popupAdmin > button {
  position: absolute;
  right: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
  width: fit-content;
  background: #9bf00b;
  font-weight: 700;
  font-size: 16px;
  line-height: 19px;
  color: #054b16;
}

i {
  font-size: 15px;
  text-align: center;
  border-radius: 50%;
  padding: 2px 8px;
  background-color: coral;
}

.popupAdmin {
  position: relative;
  height: 80%;
  width: 40%;
  display: flex;
  flex-direction: column;
  align-self: center;
  background-color: rgb(255, 255, 255);
  border-radius: 2%;
  opacity: 1;
  padding: 30px;
  gap: 30px;
}

.popupAdmin > h2 {
  text-align: right;
}

.popupAdmin__check {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px;
}
</style>
