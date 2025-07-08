<template>
  <main class="g-main-container">
    <GHeader title="Запомни слово">
      <template #right>
        <GScore :model-value="totalScore" />
      </template>
    </GHeader>

    <GButton>Начать игру</GButton>
    <div class="g-cards-container">
      <GCard v-for="card in cards" :key="card.id" :data="card" @flip="onCardFlip" />
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue';
import GButton from './components/GButton.vue';
import GHeader from './components/GHeader.vue';
import GScore from './components/GScore.vue';
import GCard from './components/GCard.vue';
import { CARD_STATE_CLOSED_VALUE, CARD_STATE_OPENED_VALUE, CARD_STATUS_FAILED_VALUE, CARD_STATUS_PENDING_VALUE, CARD_STATUS_SUCCESS_VALUE } from './constants';

const totalScore = ref(100);
const cards = ref([
  {
    id: 1,
    word: 'apple',
    translation: 'яблоко',
    state: CARD_STATE_CLOSED_VALUE,
    status: CARD_STATUS_SUCCESS_VALUE,
  },
  {
    id: 2,
    word: 'banana',
    translation: 'банан',
    state: CARD_STATE_CLOSED_VALUE,
    status: CARD_STATUS_FAILED_VALUE,
  },
  {
    id: 3,
    word: 'orange',
    translation: 'апельсин',
    state: CARD_STATE_CLOSED_VALUE,
    status: CARD_STATUS_PENDING_VALUE,
  },
])

function onCardFlip(cardId) {
  const card = cards.value.find(card => card.id === cardId)
  card.state = CARD_STATE_OPENED_VALUE
}
</script>


<style scoped></style>
