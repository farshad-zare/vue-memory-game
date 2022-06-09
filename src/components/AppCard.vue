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
    <div v-if="visible" class="card-face is-front">
      {{ position }} - {{ value }}
      <img
        class="check-icon"
        v-if="matched"
        src="/images/checkmark.svg"
        alt="matched card"
      />
    </div>
    <div v-else class="card-face is-back"></div>
  </div>
</template>

<style scoped>
.card {
  position: relative;
}

.card-face {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  border-radius: 10px;
}

.card-face.is-front {
  background-image: url(/images/card-bg.png);
}

.card-face.is-back {
  background-image: url(/images/card-bg-empty.png);
}

.check-icon {
  position: absolute;
  bottom: 5px;
  right: 5px;
}
</style>
