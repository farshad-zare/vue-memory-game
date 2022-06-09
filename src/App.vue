<script setup lang="ts">
import { ref, watch, computed } from "vue";
import type { Ref } from "vue";
import _ from "lodash";

import AppCard from "./components/AppCard.vue";
import type { CardInterface, CardPayload } from "./components/AppCard.vue";

type FlipCard = (payload: CardPayload) => void;

const cardList: Ref<CardInterface[]> = ref([]);
const userSelection: Ref<CardPayload[]> = ref([]);

const remainingPairs = computed(() => {
  const remaining = cardList.value.filter((card) => !card.matched).length / 2;
  return remaining;
});

const status = computed(() => {
  if (remainingPairs.value === 0) {
    return "you win";
  } else {
    return `${remainingPairs.value} pairs remaind`;
  }
});

const flipCard: FlipCard = (payload) => {
  cardList.value[payload.position].visible = true;

  if (userSelection.value[0]) {
    userSelection.value[1] = payload;
  } else {
    userSelection.value[0] = payload;
  }
};

const shuffleCards = () => {
  cardList.value = _.shuffle(cardList.value);
};

const restartGame = () => {
  shuffleCards();
  cardList.value = cardList.value.map((card, index) => {
    return {
      ...card,
      matched: false,
      visible: false,
      position: index,
    };
  });
};

for (let i = 0; i < 16; i++) {
  cardList.value.push({
    value: i,
    visible: true,
    position: i,
    matched: false,
  });
}

watch(
  userSelection,
  (currentValue) => {
    if (currentValue.length === 2) {
      const cardOne = currentValue[0];
      const cardTwo = currentValue[1];

      if (cardOne.faceValue === cardTwo.faceValue) {
        cardList.value[cardOne.position].matched = true;
        cardList.value[cardTwo.position].matched = true;
      } else {
        cardList.value[cardOne.position].visible = false;
        cardList.value[cardTwo.position].visible = false;
      }

      userSelection.value.length = 0;
    }
  },
  { deep: true }
);
</script>

<template>
  <h1 class="app-title">Memory Game</h1>
  <section class="game-board">
    <AppCard
      v-for="card in cardList"
      :value="card.value"
      :key="card.value"
      :visible="card.visible"
      :position="card.position"
      :matched="card.matched"
      @card-select="flipCard"
    />
  </section>
  <h2>{{ status }}</h2>
  <button @click="restartGame">Start Game</button>
</template>

<style>
.app-title {
  text-align: center;
  color: #444;
}
.game-board {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  grid-template-rows: repeat(4, 100px);
  gap: 30px;
  justify-content: center;
}
</style>
