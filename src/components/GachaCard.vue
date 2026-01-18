<script setup>
import { computed } from 'vue'
import titleCommonImg from '../assets/image/title-common.png'
import titleRareImg from '../assets/image/title-rare.png'
import titleLegendaryImg from '../assets/image/title-legendary.png'
import congratCommonImg from '../assets/image/congrat-common.png'
import congratRareImg from '../assets/image/congrat-rare.png'
import congratLegendaryImg from '../assets/image/congrate-legendary.png'
import specialNewImg from '../assets/image/special-new.png'
import lightImg from '../assets/image/light.png'
import ItemGrid from './ItemGrid.vue'

const props = defineProps({
  isOpened: Boolean,
  rarity: String,
})

const emit = defineEmits(['click'])

const titleImg = computed(() => {
  if (props.rarity === 'legendary') return titleLegendaryImg
  if (props.rarity === 'rare') return titleRareImg
  return titleCommonImg
})

const headerImg = computed(() => {
  if (props.rarity === 'legendary') return congratLegendaryImg
  if (props.rarity === 'rare') return congratRareImg
  return congratCommonImg
})
</script>

<template>
  <div class="card" :class="{ opened: isOpened, [rarity]: isOpened }" @click="emit('click')">
    <img v-if="!isOpened" src="../assets/image/Back-Card.png" class="back-card-img" alt="Card Back" />

    <img v-if="isOpened && rarity === 'legendary'" class="special-badge" :src="specialNewImg" alt="Special New" />

    <div v-if="isOpened" class="card-content">
      <img class="card-title-img" :src="titleImg" />
      <img class="card-header-img" :src="headerImg" />
        <img v-if="rarity === 'legendary'" class="light-effect" :src="lightImg" alt="Light Effect" />

      <ItemGrid />

      <div class="collect-label">COLLECT YOUR REWARDS</div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.card {
  width: 340px;
  height: 672px;
  position: relative;
  cursor: pointer;
  transition: transform 0.3s ease;

  &:hover {
    transform: scale(1.02);
  }

  .back-card-img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

  &.opened {
    background: linear-gradient(180deg, rgba(0, 0, 0, 0.8) 0%, rgba(0, 27, 43, 0.8) 100%);
    border: 1.5px solid #2889ff;
    border-radius: 4px 4px 40px 4px;
    padding: 24px;
    display: flex;
    flex-direction: column;
    align-items: center;
    box-shadow: 0px 0px 30px rgba(0, 0, 0, 0.5);

    &.rare {
      background: linear-gradient(180deg, rgba(0, 0, 0, 0.8) 0%, rgba(15, 0, 37, 0.8) 100%);
      border-color: #8b3bff;
      box-shadow: 0px 0px 30px rgba(139, 59, 255, 0.2);
    }

    &.legendary {
      background-image: url('../assets/image/light.png');
     background: linear-gradient(0deg, rgba(37, 27, 0, 0.5), rgba(37, 27, 0, 0.5)),
     linear-gradient(0deg, rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0.8));
      background-size: 653px 924px, 653px 924px, cover;
      background-position: center, center, center;
      background-repeat: no-repeat;
      border-color: #ffc017;
      box-shadow: 0px 0px 40px rgba(255, 192, 23, 0.3);
    }
  }

  .special-badge {
    position: absolute;
    top: -12px;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 240px;
    z-index: 20;
    pointer-events: none;
  }
        .light-effect {
      width: 653px;
      height: 924px;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      object-fit: contain;
      pointer-events: none;
      z-index: 0;
      opacity: 0.1;
    }


  .card-content {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;

    .card-title-img {
      height: 32px;
      object-fit: contain;
    }

    .card-header-img {
      height: 70px;
      object-fit: contain;
    }
    .collect-label {
      margin-top: auto;
      color: rgba(255, 255, 255, 0.3);
      font-size: 12px;
      letter-spacing: 1px;
    }
  }
}
</style>
