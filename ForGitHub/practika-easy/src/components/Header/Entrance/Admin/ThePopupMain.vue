<script setup>
import { provide, reactive, ref } from 'vue';

import Title from './TheTitle.vue';
import SubTitle from './TheSubTitle.vue';
import AdminPanel from './AdminPanel/TheAdminPanel.vue';

const section = ref('');
const subSection = ref('');

const data = reactive({});

provide('section', section);
provide('subSection', subSection);
provide('dataAdmin', data);
</script>

<template>
  <div class="popupAdmin__main">
    <div class="popupAdmin-sections">
      <Title :selected-section="section" @select-section="(s) => (section = s)" />
      <!-- prettier-ignore -->
      <SubTitle :selected-sub="subSection" @select-sub="(s) => (subSection = s)" v-if="section" />
    </div>
    <AdminPanel v-if="section && subSection" />
  </div>
</template>

<style scoped>
.popupAdmin-sections {
  display: flex;
  flex-direction: column;
  justify-content: start;
  gap: 10px;
  margin-bottom: 20px;
}
</style>
