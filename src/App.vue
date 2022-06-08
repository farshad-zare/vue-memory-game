<script setup lang="ts">
import { ref } from "vue";
import type { Ref } from "vue";

import AppCard from "./components/AppCard.vue";
import type { CardInterface } from "./components/AppCard.vue";

type FlipCard = (payload: number) => void;

const cardList: Ref<CardInterface[]> = ref([]);

const flipCard: FlipCard = (num) => {
  cardList.value[num].visible = true;
};

for (let i = 0; i < 16; i++) {
  cardList.value.push({ value: i, visible: false, position: i });
}
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
