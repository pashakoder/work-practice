<script setup>
const { selectedSub } = defineProps(['selectedSub']);

defineEmits(['selectSub']);

const sectionValues = ['Users', 'News', 'Comments', 'Games', 'Img'];
</script>

<template>
  <section class="sections__sub">
    <button
      v-for="section in sectionValues"
      :key="section"
      :class="['sections__sub-btn', { active: section === selectedSub }]"
      @click="$emit('selectSub', section)"
    >
      {{ section }}
    </button>
  </section>
</template>

<style scoped>
.sections__sub {
  display: flex;
  gap: 20px;
}

/* 
//
//  Button Style
//
*/

.sections__sub-btn {
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

.sections__sub-btn.active {
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
