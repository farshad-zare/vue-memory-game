<script setup lang="ts">
defineProps<CardInterface>();

const emit = defineEmits<{
  (e: "card-select", payload: CardPayload): number;
}>();
</script>
<!-- new script tag couse cant import interface from other files
and cant export interface from script setup -->
<script lang="ts">
type CardInterface = {
  value: string;
  position: number;
  visible: boolean;
  matched: boolean;
  variant?: 1 | 2;
};

interface CardPayload {
  position: number;
  faceValue: string;
}

export type { CardInterface, CardPayload };
</script>

<template>
  <div
    class="card"
    :class="visible ? 'is-flipped' : null"
    @click="
      () => {
        if (visible) {
          return;
        }
        emit('card-select', { position, faceValue: value });
      }
    "
  >
    <div class="card-face is-front">
      <img class="card-img" :src="`/images/${value}.png`" :alt="value" />
      <img
        class="check-icon"
        v-if="matched"
        src="/images/checkmark.svg"
        alt="matched card"
      />
    </div>
    <div class="card-face is-back"></div>
  </div>
</template>

<style scoped>
.card {
  position: relative;
  transition-property: transform;
  transition-duration: 0.5s;
  transition-timing-function: ease-in;
  transform-style: preserve-3d;
  box-shadow: 0 0 10px 1px rgba(0, 0, 0, 0.63);
  border-radius: 10px;
}
.card.is-flipped {
  transform: rotateY(180deg);
}

.card-face {
  border-radius: 10px;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  backface-visibility: hidden;
}

.card-face.is-front {
  background-image: url(/images/card-bg.png);
  transform: rotateY(180deg);
}

.card-face.is-back {
  background-image: url(/images/card-bg-empty.png);
  cursor: pointer;
}

.check-icon {
  position: absolute;
  bottom: 5px;
  right: 5px;
}

.card-img {
  width: 100%;
  height: 100%;
}
</style>
