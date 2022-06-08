<script setup lang="ts">
import { ref, watch } from "vue";
import type { Ref } from "vue";

import AppCard from "./components/AppCard.vue";
import type { CardInterface, CardPayload } from "./components/AppCard.vue";

type FlipCard = (payload: CardPayload) => void;

const cardList: Ref<CardInterface[]> = ref([]);
const userSelection: Ref<CardPayload[]> = ref([]);

const flipCard: FlipCard = (payload) => {
  cardList.value[payload.position].visible = true;

  if (userSelection.value[0]) {
    userSelection.value[1] = payload;
  } else {
    userSelection.value[0] = payload;
  }
};

for (let i = 0; i < 16; i++) {
  cardList.value.push({ value: i, visible: false, position: i });
}

watch(
  userSelection,
  (currentValue) => {
    if (currentValue.length === 2) {
      const cardOne = currentValue[0];
      const cardTwo = currentValue[1];

      cardList.value[cardOne.position].visible = false;
      cardList.value[cardTwo.position].visible = false;

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
      v-for="(card, index) in cardList"
      :value="card.value"
      :key="card.value"
      :visible="card.visible"
      :position="index"
      @card-select="flipCard"
    />
  </section>
  <h2>{{ userSelection }}</h2>
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
