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
  <div class="card" :class="{ 'is-flipped': isOpened }" @click="emit('click')">
    <div class="card-inner">

      <div class="card-face card-back">
        <img src="../assets/image/Back-Card.png" class="back-card-img" alt="Card Back" />
      </div>

      <div class="card-face card-front" :class="[rarity]">

        <img v-if="rarity === 'legendary'" class="special-badge" :src="specialNewImg" alt="Special New" />

        <div class="border-layer" :class="[rarity]">

          <div class="content-layer" :class="[rarity]">
            <div class="card-content">
              <img class="card-title-img" :src="titleImg" />
              <img class="card-header-img" :src="headerImg" />
              <img v-if="rarity === 'legendary'" class="light-effect" :src="lightImg" alt="Light Effect" />

              <ItemGrid />

              <div class="collect-label">COLLECT YOUR REWARDS</div>
            </div>
          </div>

        </div>

      </div>

    </div>
  </div>
</template>

<style scoped lang="scss">
$cut-size: 30px;
$border-width: 2px;


@mixin card-clip-path {
  clip-path: polygon(0 0,
    100% 0,
    100% calc(100% - #{$cut-size}),
    calc(100% - #{$cut-size}) 100%,
    0 100%);
}

.card {
  width: 340px;
  height: 672px;
  perspective: 1000px;
  position: relative;
  cursor: pointer;
  background: transparent;

  &:hover {
    transform: scale(1.02);
    transition: transform 0.3s ease;
  }

  .card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
    transform-style: preserve-3d;
  }

  &.is-flipped .card-inner {
    transform: rotateY(180deg);
  }

  .card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
  }

  .card-back {
    z-index: 2;
    transform: rotateY(0deg);
    border-radius: 4px;
    overflow: hidden;

    .back-card-img {
      width: 100%;
      height: 100%;
      object-fit: none;
    }
  }


  .card-front {
    transform: rotateY(180deg);
    background: transparent;
    border: none;


    filter: drop-shadow(0px 0px 20px rgba(0, 0, 0, 0.6));


    overflow: visible;

    &.rare {
      filter: drop-shadow(0px 0px 20px rgba(139, 59, 255, 0.4));
    }

    &.legendary {
      filter: drop-shadow(0px 0px 25px rgba(255, 192, 23, 0.5));
    }
  }


  .border-layer {
    width: 100%;
    height: 100%;


    background-color: #2889ff;


    padding: $border-width;


    @include card-clip-path;

    &.rare {
      background-color: #8b3bff;
    }

    &.legendary {
      background-color: #ffc017;
    }
  }


  .content-layer {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 24px;


    background: linear-gradient(180deg, rgba(0, 0, 0, 0.8) 0%, rgba(0, 27, 43, 0.8) 100%);


    @include card-clip-path;

    &.rare {
      background: linear-gradient(180deg, rgba(0, 0, 0, 0.8) 0%, rgba(15, 0, 37, 0.8) 100%);
    }

    &.legendary {
      background: linear-gradient(0deg, rgba(37, 27, 0, 0.5), rgba(37, 27, 0, 0.5)),
        linear-gradient(0deg, rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0.8));
    }
  }

  .special-badge {
    position: absolute;
    top: -47px;
    left: 15%;
    width: 240px;
    z-index: 20;
  }

  .card-content {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 12px;
    height: 100%;

    .card-title-img {
      height: 32px;
      object-fit: contain;
    }

    .card-header-img {
      height: 70px;
      object-fit: contain;
    }

    .light-effect {
      width: 653px;
      height: 924px;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      opacity: 0.1;
      pointer-events: none;
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