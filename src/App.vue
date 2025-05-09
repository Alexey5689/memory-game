<script setup lang="ts">

import { ref, onMounted, computed, watch } from 'vue';
import MemoryCard from './components/MemoryCard.vue';

const symbols = [
  { id: 1, emoji: "🐶", name: "Собака" },
  { id: 2, emoji: "🐱", name: "Кошка" },
  { id: 3, emoji: "🐭", name: "Мышь" },
  { id: 4, emoji: "🐹", name: "Хомяк" },
  { id: 5, emoji: "🐰", name: "Кролик" },
  { id: 6, emoji: "🦊", name: "Лиса" },
  { id: 7, emoji: "🐻", name: "Медведь" },
  { id: 8, emoji: "🐼", name: "Панда" },
];

const cards:any = ref([]);

const matchedCards = ref(new Set<number>());
const openCards = ref(new Set<number>())
const totalPairs = symbols.length * 2
const matched = computed(()=>matchedCards.value.size)
const hasTwoCardsOpened = computed(()=>openCards.value.size===2);
const move = ref<number>(0)
const isGamewon = computed(()=>matched.value === totalPairs)
function resetGame(){
    move.value = 0;
    openCards.value.clear();
    matchedCards.value.clear();
    cards.value = [...symbols, ...symbols].sort(() => Math.random() - 0.5);
}

function getStatus(index:number){
    if(openCards.value.has(index)){
        return 'opened';
    }
    if(matchedCards.value.has(index)){
        return 'matched';
    }
    return 'closed';
}
const CLOSE_TIMEOUT = 1000;
const openCard = (index: number) => {
  if (getStatus(index) !== 'closed' || hasTwoCardsOpened.value) return;
  move.value++;
  openCards.value.add(index);
};

watch(hasTwoCardsOpened, (areTwoCardsOpen) => {
  if (!areTwoCardsOpen) return;
  
  const [firstIndex, secondIndex] = [...openCards.value];
  const isMatch = cards.value[firstIndex].id === cards.value[secondIndex].id;

  setTimeout(() => {
    if (isMatch) {
      matchedCards.value.add(firstIndex);
      matchedCards.value.add(secondIndex);
    }
    openCards.value.clear();
  }, CLOSE_TIMEOUT);
});
onMounted(resetGame);
</script>
<template>
  <div class="memory-game">
    <h1>Memory Game</h1>
    <div class="controls">
      <button @click="resetGame">Новая игра</button>
      <p>Попытки: {{ move }}</p>
      <p>Совпадения: {{ matched }}/ {{ totalPairs }}</p>
    </div>
    <div class="cards-grid">
      <MemoryCard @click="openCard(index)" v-for="(card, index) in cards" :disabled="hasTwoCardsOpened" :key="index" :status="getStatus(index)" :image="card.emoji"/>
    </div>
     <h1 v-if="isGamewon">You won!!!</h1>
  </div>
</template>
<style scoped>
.memory-game {
  text-align: center;
  padding: 20px;
}

.controls {
  margin-bottom: 20px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  max-width: 500px;
  margin: 0 auto;
}


</style>