<script setup>
const { selectedSection } = defineProps(['selectedSection']);

defineEmits(['selectSection']);

const sectionValues = ['Добавить', 'Изменить'];
</script>

<template>
  <section class="sections">
    <button
      v-for="section in sectionValues"
      :key="section"
      :class="['section-btn', { active: section === selectedSection }]"
      @click="$emit('selectSection', section)"
    >
      {{ section }}
    </button>
  </section>
</template>

<style scoped>
.sections {
  display: flex;
  gap: 20px;
}

/* 
//
//  Button Style
//
*/

.section-btn {
  border-radius: 30px;
  padding: 5px 10px;
  transition:
    background 0.3s ease-in-out,
    color 0.3s ease-in-out,
    font-weight 0.3s ease-in-out;
}

.section-btn:not(.active):hover {
  opacity: 0.6;
}

.section-btn.active {
  background: #9bf00b;
  border-radius: 30px;
  cursor: default;
  color: #054b16;
  font-weight: bold;
}

@keyframes active-animation {
  0% {
    background: transparent;
    color: initial;
    font-weight: normal;
  }
  100% {
    background: #9bf00b;
    color: #054b16;
    font-weight: bold;
  }
}

button:hover {
  opacity: 1;
}
</style>
