<template>
  <main class="g-main-container">
    <GHeader title="Запомни слово">
      <template #right>
        <GScore :model-value="totalScore" />
      </template>
    </GHeader>

    <GButton>Начать игру</GButton>
    <div class="g-cards-container" v-if="cards?.length > 0">
      <GCard v-for="card in cards" :key="card.id" :data="card" @flip="onCardFlip" />
    </div>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import GButton from './components/GButton.vue';
import GHeader from './components/GHeader.vue';
import GScore from './components/GScore.vue';
import GCard from './components/GCard.vue';
import { CARD_STATE_CLOSED_VALUE, CARD_STATE_OPENED_VALUE, CARD_STATUS_FAILED_VALUE, CARD_STATUS_PENDING_VALUE, CARD_STATUS_SUCCESS_VALUE } from './constants';

const totalScore = ref(100);
const cards = ref(null)

onMounted(() => {
  getCardsDataFromApi()
})

async function getCardsDataFromApi() {
  try {
    const response = await fetch('http://localhost:8080/api/random-words')
    const data = await response.json()

    if (data && data.length > 0) {
      cards.value = data.map(({ word, translation }, index) => ({
        id: index + 1,
        word,
        translation,
        state: CARD_STATE_CLOSED_VALUE,
        status: CARD_STATUS_PENDING_VALUE,
      }))
    }
  } catch (error) {
    console.error(error);
  }

}

function onCardFlip(cardId) {
  const card = cards.value.find(card => card.id === cardId)
  card.state = CARD_STATE_OPENED_VALUE
}
</script>


<style scoped>
.g-cards-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  row-gap: 4.125rem;
  justify-items: center;
}
</style>
