<template>
  <main class="g-main-container">
    <GHeader title="Запомни слово">
      <template #right>
        <GScore :model-value="100" />
      </template>
    </GHeader>

    <GButton>Начать игру</GButton>

    <GCard />
  </main>
</template>

<script setup>
import GButton from './components/GButton.vue';
import GHeader from './components/GHeader.vue';
import GScore from './components/GScore.vue';
import GCard from './components/GCard.vue';
</script>


<style scoped></style>
