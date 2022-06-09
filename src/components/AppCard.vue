<script setup lang="ts">
defineProps<CardInterface>();

const emit = defineEmits<{
  (e: "card-select", payload: CardPayload): number;
}>();
</script>
<!-- new script tag couse cant import interface from other files
and cant export interface from script setup -->
<script lang="ts">
interface CardInterface {
  value: number;
  position: number;
  visible: boolean;
  matched: boolean;
}

interface CardPayload {
  position: number;
  faceValue: number;
}

export type { CardInterface, CardPayload };
</script>

<template>
  <div
    class="card"
    @click="emit('card-select', { position, faceValue: value })"
  >
    <div v-if="visible" class="card-face is-front">{{ value }}</div>
    <div v-else class="card-face is-back">back</div>
  </div>
</template>

<style scoped>
.card {
  border: 3px solid #ccc;
  position: relative;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
}

.card-face.is-front {
  background-color: red;
}

.card-face.is-back {
  background-color: blue;
}
</style>
