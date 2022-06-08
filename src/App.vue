<script setup lang="ts">
import AppCard from "./components/AppCard.vue";
</script>

<template>
  <h1 class="app-title">Memory Game</h1>
  <section class="game-board">
    <AppCard />
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
