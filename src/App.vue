<script setup>
import { ref } from 'vue'
import GachaCard from './components/GachaCard.vue'

const openedCardIndex = ref(new Set())
const cardRarity = ref([null, null, null])

function getRandomRarity() {
  const rarities = ['common', 'rare', 'legendary']
  const weights = [30, 30, 40]
  const random = Math.random() * 100

  if (random < weights[0]) return rarities[0]
  if (random < weights[0] + weights[1]) return rarities[1]
  return rarities[2]
}

function handleCardClick(index) {
  const newSet = new Set(openedCardIndex.value)
  if (newSet.has(index)) {
    newSet.delete(index)
  } else {
    newSet.add(index)
    if (!cardRarity.value[index]) {
      cardRarity.value[index] = getRandomRarity()
    }
  }
  openedCardIndex.value = newSet
}
</script>

<template>
  <div class="fs">
    <img class="logo" src="./assets/image/Logo.png" alt="Logo" />

    <div class="body-wrapper">
      <GachaCard
        v-for="(n, index) in 3"
        :key="index"
        :is-opened="openedCardIndex.has(index)"
        :rarity="cardRarity[index]"
        @click="handleCardClick(index)"
      />
    </div>
  </div>
</template>

<style scoped lang="scss">
.fs {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background-image: url('./assets/image/bg.png');
  background-size: cover;
  background-position: center;
  .logo {
    position: fixed;
    top: 32px;
    right: 32px;
    width: 56px;
    z-index: 100;
  }

  .body-wrapper {
    display: flex;
    gap: 32px;
    align-items: center;
  }
}
</style>
