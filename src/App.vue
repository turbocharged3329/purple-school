<template>
  <main class="g-main-container">
    <GHeader title="Запомни слово">
      <template #right>
        <GScore :model-value="100" />
      </template>
    </GHeader>

    <GButton>Начать игру</GButton>
  </main>
</template>

<script setup>
import GButton from './components/GButton.vue';
import GHeader from './components/GHeader.vue';
import GScore from './components/GScore.vue';
</script>


<style scoped></style>
