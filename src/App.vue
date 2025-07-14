<template>
  <main class="g-main-container">
    <GHeader title="Запомни слово">
      <template #right>
        <GScore :model-value="totalScore" />
      </template>
    </GHeader>

    <GButton class="g-start-game-button" v-if="!isGameStarted" @click="onStartGame">Начать игру</GButton>

    <template v-else>
      <template v-if="cards?.length">
    <div class="g-cards-container" >
      <GCard v-for="card in cards" :key="card.id" :data="card" @flip="onCardFlip" @accept="onCardAccept(card.id)" @reject="onCardReject(card.id)  " />
    </div>
  </template>
  </template>
  <div class="g-footer">
  <GButton v-if="isGameStarted && cards?.length" @click="onRestartGame">Начать заново</GButton>
</div>
  </main>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import GButton from './components/GButton.vue';
import GHeader from './components/GHeader.vue';
import GScore from './components/GScore.vue';
import GCard from './components/GCard.vue';
import { CARD_STATE_CLOSED_VALUE, CARD_STATE_OPENED_VALUE, CARD_STATUS_PENDING_VALUE, CARD_STATUS_SUCCESS_VALUE, CARD_STATUS_FAILED_VALUE } from './constants';

const isGameStarted = ref(false)
const totalScore = ref(100);
const cards = ref(null)

onMounted(() => {
  getCardsDataFromApi()
})

function onStartGame() {
  isGameStarted.value = true
}

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

function onCardAccept(cardId) {
  const card = cards.value.find(card => card.id === cardId)
  card.status = CARD_STATUS_SUCCESS_VALUE
  totalScore.value += 10
}

function onCardReject(cardId) {
  const card = cards.value.find(card => card.id === cardId)
  card.status = CARD_STATUS_FAILED_VALUE
  totalScore.value -= 4
}

function onRestartGame() {
  getCardsDataFromApi()
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  })
}
</script>


<style scoped>
.g-cards-container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  row-gap: 4.125rem;
  padding: 0 4.125rem;
  min-height: 100vh;
  position: relative;
}

.g-start-game-button {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.g-footer {
  display: flex;
  justify-content: center;
  padding: 4.125rem 0;
}
</style>
