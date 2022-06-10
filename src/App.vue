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
    return `${remainingPairs.value} pairs remains`;
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
  userSelection.value.length = 0;
  cardList.value = cardList.value.map((card, index) => {
    return {
      ...card,
      matched: false,
      visible: false,
      position: index,
    };
  });
};

const cardItem = [
  "bat",
  "candy",
  "cauldron",
  "cupcake",
  "ghost",
  "moon",
  "pumpkin",
  "witch-hat",
];

cardItem.forEach((item, index) => {
  cardList.value.push({
    value: item,
    visible: false,
    position: index,
    matched: false,
    variant: 1,
  });
  cardList.value.push({
    value: item,
    visible: false,
    position: index,
    matched: false,
    variant: 2,
  });
});

cardList.value = cardList.value.map((card, index) => {
  return {
    ...card,
    position: index,
  };
});

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
        setTimeout(() => {
          cardList.value[cardOne.position].visible = false;
          cardList.value[cardTwo.position].visible = false;
        }, 1500);
      }

      userSelection.value.length = 0;
    }
  },
  { deep: true }
);
</script>

<template>
  <h1 class="app-title">Memory Game</h1>
  <button class="start-btn" @click="restartGame">Start Game</button>
  <transition-group tag="section" name="shuffle-card" class="game-board">
    <AppCard
      v-for="card in cardList"
      :value="card.value"
      :key="`${card.variant}-${card.value}`"
      :visible="card.visible"
      :position="card.position"
      :matched="card.matched"
      @card-select="flipCard"
    />
  </transition-group>
  <h2>{{ status }}</h2>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  border: 0;
  box-sizing: border-box;
}

#app {
  background-image: url(images/page-bg.png);
  background-color: #00070c;
  min-height: 100vh;
  color: #fff;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}

.app-title {
  text-align: center;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(4, 100px);
  grid-template-rows: repeat(4, 100px);
  gap: 20px;
  justify-content: center;
}

.start-btn {
  background-color: orange;
  font-weight: 900;
  padding: 10px;
  border-radius: 10px;
}

.start-btn:hover {
  cursor: pointer;
}

.shuffle-card-move {
  transition-property: transform;
  transition-duration: 0.5s;
  transition-timing-function: ease-in;
}
</style>
