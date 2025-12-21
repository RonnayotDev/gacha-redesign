<template>
  <div
    class="relative w-full h-screen bg-gray-900 overflow-hidden text-white font-sans selection:bg-blue-500 selection:text-white">
    <img :src="bgImage" alt="Background" class="absolute inset-0 w-full h-full object-cover z-0" />

    <div class="absolute inset-0 sunburst opacity-60 animate-spin-slow z-1"></div>
    <div class="absolute inset-0 bg-radial-gradient from-transparent via-black/60 to-black z-1"></div>

    <div v-if="!showResultModal"
      class="absolute bottom-2 left-2 md:bottom-4 md:left-4 z-[100] opacity-90 pointer-events-none">
      <img :src="hudImg" alt="HUD" class="h-10 md:h-14 w-auto drop-shadow-lg" />
    </div>

    <div class="absolute top-4 right-4 md:top-6 md:right-6 z-[200] flex items-end gap-3">
      <div v-if="!showResultModal"
        class="flex items-center gap-3 bg-black rounded-full px-4 py-2 md:px-6 md:py-3 border border-gray-800 shadow-xl">
        <div class="w-6 h-6 md:w-8 md:h-8 flex items-center justify-center">
          <img src="../assets/img/Lucky_wheel.png" alt="Icon"
            class="w-full h-full object-contain filter drop-shadow-md" />
        </div>
        <span class="text-white font-bold text-sm md:text-xl tracking-wide font-sans">
          Remaining spin : {{ remainingSpins }}
        </span>
      </div>
      <img :src="logoLuvImg" alt="Logo LUV"
        class="w-14 md:w-14 h-auto object-contain drop-shadow-xl filter hover:brightness-110 transition-all" />
    </div>

    <div class="relative z-10 w-full h-full flex flex-col">
      <div class="flex-1 flex flex-col items-center justify-center px-4 transition-all duration-500"
        :class="showWheelModal ? 'blur-sm scale-95 opacity-50' : ''">

        <div class="text-center space-y-4 mb-8">
          <h1
            class="text-5xl md:text-7xl font-display font-black tracking-tighter text-white drop-shadow-[0_0_15px_rgba(255,255,255,0.5)] flex justify-center items-center">
            <img src="../assets/img/Logo.png" alt="Logo" class="max-h-24 md:max-h-32" />
          </h1>
          <p class="text-white-300/80 text-sm md:text-base italic max-w-2xl mx-auto">
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolorem
            iure explicabo fuga.
          </p>
        </div>

        <div class="flex flex-wrap justify-center gap-2 md:gap-3 py-8 max-w-5xl">
          <div v-for="(item, index) in targetWord" :key="index" class="group relative">
            <button @click="handleSlotClick(index)" :disabled="item.collected || remainingSpins <= 0"
              class="w-12 h-16 md:w-20 md:h-24 flex items-center justify-center rounded-lg border-2 transition-all duration-300 transform p-4"
              :class="[
                item.collected
                  ? 'bg-blue-600/20 border-blue-400 shadow-[0_0_20px_rgba(59,130,246,0.6)] scale-100'
                  : remainingSpins <= 0
                    ? 'bg-gray-800/50 border-gray-700 opacity-50 cursor-not-allowed'
                    : 'bg-black/40 border-white/10 text-transparent hover:border-white/30 hover:-translate-y-1 hover:bg-white/5 cursor-pointer',
              ]">
              <img v-if="item.image" :src="item.image"
                class="w-full h-full object-contain p-1 transition-all duration-500" :class="item.collected
                  ? 'opacity-100 scale-100'
                  : 'opacity-0 scale-50'
                  " />
              <span v-else class="text-4xl md:text-6xl font-balloon transition-all duration-500" :class="[
                item.collected
                  ? 'opacity-100 scale-100'
                  : 'opacity-0 scale-50',
                index % 3 === 0
                  ? 'text-balloon-blue'
                  : index % 3 === 1
                    ? 'text-balloon-purple'
                    : 'text-balloon-cyan',
              ]">
                {{ item.char }}
              </span>
            </button>
          </div>
        </div>

        <div class="mt-8 text-center">
          <button
            class="flex items-center justify-center w-[200px] h-[44px] rounded-[4px] px-2 gap-2 border-2 transition-all duration-300 font-bold text-base tracking-wider mx-auto"
            :class="[
              isAllCollected
                ? 'bg-[#2098E2]/80 border-white/10 shadow-[0_0_10px_0_#2098E240] text-white hover:bg-[#2098E2] hover:scale-105 active:scale-95 cursor-pointer'
                : 'bg-gray-800 text-gray-500 border-gray-700 cursor-not-allowed opacity-60'
            ]" :disabled="!isAllCollected" @click="openWheelModal">
            CREATE NOW!
          </button>

          <p v-if="remainingSpins <= 0 && !showWheelModal && !showResultModal"
            class="text-red-400 mt-4 text-sm font-bold animate-pulse">
            * สิทธิ์หมุนของคุณหมดแล้ว ไม่สามารถสะสมตัวอักษรเพิ่มได้
          </p>
        </div>
      </div>
    </div>

    <div v-if="showWheelModal"
      class="fixed inset-0 z-50 flex items-center justify-center backdrop-blur-sm p-4 transition-all duration-500">
      <div class="relative flex flex-col items-center max-w-2xl w-full">
        <h2
          class="text-3xl md:text-5xl font-display font-black text-white mb-6 md:mb-10 drop-shadow-[0_0_15px_rgba(59,130,246,0.8)] tracking-wider">
          <img src="../assets/img/Logo.png" alt="" />
        </h2>

        <div class="relative w-[450px] h-[450px] md:w-[700px] md:h-[700px] flex items-center justify-center">

          <div class="absolute inset-0 z-0 flex items-center justify-center pointer-events-none">
            <img :src="circleBlueImg"
              class="w-full h-full object-contain scale-90 border-[6px] border-[#79CCFF] rounded-full"
              alt="Blue Glow" />
          </div>

          <div class="absolute inset-0 z-10 flex items-center justify-center pointer-events-none">
            <img :src="wrapperImg" class="w-[115%] h-[115%] object-contain drop-shadow-xl scale-110" alt="Wrapper" />
          </div>

          <div
            class="absolute -top-6 md:-top-10 left-1/2 -translate-x-1/2 z-40 w-20 h-24 md:w-32 md:h-40 filter drop-shadow-xl">
            <img :src="pinImg" class="w-full h-full object-contain" alt="Pin" />
          </div>

          <div
            class="relative z-20 w-[340px] h-[340px] md:w-[540px] md:h-[540px] rounded-full border-[8px] border-[#969696]/20 overflow-hidden transition-transform duration-[3000ms] ease-spin-fast bg-[#282828]"
            style="box-shadow: 0px 0px 50px rgba(0, 0, 0, 0.5);" :style="{ transform: `rotate(${wheelRotation}deg)` }">

            <div v-for="(reward, index) in rewards" :key="'bg-' + reward.id"
              class="absolute top-0 left-1/2 w-1/2 h-[50%] origin-bottom-left border-l border-r border-transparent"
              :style="{
                transform: `rotate(${index * 45}deg) skewY(-45deg)`,
                ...getSegmentStyle(reward.theme)
              }">
            </div>

            <div class="absolute inset-0 w-full h-full pointer-events-none">
              <div v-for="(reward, index) in rewards" :key="'content-' + reward.id"
                class="absolute top-1/2 left-1/2 w-0 h-0 flex justify-center items-center" :style="{
                  transform: `rotate(${index * 45 + 22.5}deg)`,
                }">

                <div
                  class="flex flex-col items-center justify-center text-center transform -translate-y-[125px] md:-translate-y-[200px] w-[130px] md:w-[240px]">

                  <div class="w-full flex items-center justify-center h-[50px] md:h-[100px] mb-1 md:mb-2">
                    <img :src="reward.resultImage" alt="item"
                      class="w-full h-full object-contain drop-shadow-[0_8px_8px_rgba(0,0,0,0.5)] transform rotate-90 transition-transform duration-300" />
                  </div>

                  <div class="text-center w-full flex flex-col items-center relative z-10">
                    <span
                      class="block text-[10px] md:text-[16px] font-bold text-white leading-tight drop-shadow-md break-words w-full font-line-seed px-1">
                      {{ reward.name }}
                    </span>

                    <span class="block font-line-seed font-normal text-[12px] leading-none text-[#FFFFFF80] mt-1"
                      v-if="reward.stock > 0">
                      เหลือ {{ reward.stock }}
                    </span>

                    <span class="block text-[9px] text-red-400 mt-0.5 font-bold" v-else>
                      หมด
                    </span>
                  </div>
                </div>
              </div>
            </div>

            <div
              class="absolute inset-0 m-auto w-[30%] h-[30%] rounded-full bg-[#1C1C1C] border-[6px] border-[#ffffff]/10 shadow-inner z-20 flex items-center justify-center">
            </div>
          </div>

          <div
            class="absolute inset-0 m-auto w-[30%] h-[30%] z-30 flex items-center justify-center pointer-events-none">
            <button @click="spinTheWheel" :disabled="isSpinning || remainingSpins <= 0"
              class="pointer-events-auto relative w-24 h-24 md:w-32 md:h-32 rounded-full flex items-center justify-center transition-all group hover:scale-105 active:scale-95"
              :class="remainingSpins <= 0 ? 'grayscale cursor-not-allowed' : ''" style="background: radial-gradient(63.29% 63.29% at 50% 36.71%, #79CCFF 0%, #209CE9 100%);
                            box-shadow: 0px 0px 5px rgba(255, 255, 255, 0.5), 0px 0px 75px #1C1C1C;">

              <div class="absolute inset-0 rounded-full border-[4px] border-white opacity-20"></div>

              <span class="relative font-line-seed font-black text-2xl md:text-4xl tracking-wider text-white"
                style="text-shadow: 0px 0px 20px #209CE9;">
                SPIN
              </span>
            </button>
          </div>

        </div>
      </div>
    </div>

    <div v-if="showResultModal" @click="resetGameRound"
      class="fixed inset-0 z-[60] flex items-center justify-center bg-black animate-fade-in overflow-hidden cursor-pointer">

      <div class="absolute inset-0 transition-colors duration-1000" :class="[
        wonReward?.theme === 'red' ? 'bg-red-700/40' :
          wonReward?.theme === 'green' ? 'bg-green-700/40' :
            wonReward?.theme === 'blue' ? 'bg-blue-700/40' : 'bg-gray-800/40',
      ]"></div>

      <div class="absolute inset-0 opacity-40 sunburst-ray animate-spin-slow z-1"></div>
      <div class="absolute inset-0 bg-radial-gradient-center z-1"></div>

      <div class="relative z-10 flex flex-col items-center justify-center text-center w-full max-w-4xl px-4">

        <div class="relative mb-2 flex flex-col items-center">
          <h2
            class="text-[48px] font-bold text-white text-center uppercase tracking-wide z-10 font-line-seed leading-none"
            :style="{
              textShadow: wonReward?.theme === 'red' ? '0px 0px 20px rgba(255, 0, 0, 0.5)' :
                wonReward?.theme === 'green' ? '0px 0px 20px rgba(0, 255, 0, 0.5)' :
                  wonReward?.theme === 'blue' ? '0px 0px 20px rgba(0, 0, 255, 0.5)' :
                    '0px 0px 20px rgba(255, 255, 255, 0.3)'
            }">
            CONGRATULATIONS!
          </h2>

          <p class="font-lucky-wheel whitespace-nowrap -mt-10 relative z-20" :class="[
            wonReward?.theme === 'red' ? 'text-[#209CE9]' :
              wonReward?.theme === 'green' ? 'text-green-400' :
                wonReward?.theme === 'blue' ? 'text-[#209CE9]' : 'text-[#209CE9]'
          ]">
            Lucky Wheel
          </p>
        </div>

        <p class="text-gray-200 text-lg md:text-xl font-light mb-10 mt-4">
          Wow! You've just won a
          <span class="font-bold text-white">{{ wonReward?.name }}</span>
        </p>

        <div class="relative flex items-center justify-center w-full py-10 mb-8">

          <div class="absolute w-64 h-64 rounded-full blur-[80px]" :class="[
            wonReward?.theme === 'red' ? 'bg-red-600' :
              wonReward?.theme === 'green' ? 'bg-green-500' :
                wonReward?.theme === 'blue' ? 'bg-blue-500' : 'bg-gray-600',
          ]"></div>

          <img :src="wonReward?.resultImage"
            class="relative w-64 md:w-80 h-auto object-contain drop-shadow-[0_10px_30px_rgba(0,0,0,0.8)] transform hover:scale-105 transition-transform duration-500"
            alt="Reward" />
        </div>

      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from "vue";

// --- Import Assets ---
import pinImg from "../assets/img/Pin.png";
import wrapperImg from "../assets/img/wrapper.png";
import circleBlueImg from "../assets/img/Circle-blue.png";
import hudImg from "../assets/img/HeartBar.png";
import logoLuvImg from "../assets/img/LogoLuv.png";
import bgImage from "../assets/img/Drop-frame.png";
import letterA from "../assets/img/A.png";
import letterN from "../assets/img/N.png";
import letterI from "../assets/img/I.png";
import letterV from "../assets/img/V.png";
import letterE from "../assets/img/E.png";
import letterR from "../assets/img/R.png";
import letterS from "../assets/img/S.png";
import letterY from "../assets/img/Y.png";
// New Item Image (Silver Bar)
import itemBgImg from "../assets/img/item.png";


// --- Configuration ---
const remainingSpins = ref(3);
const spinCount = ref(1);

// Reward Data
const rewards = ref([
  { id: 1, name: "iPhone 17", stock: 1, weight: 100, theme: "red", resultImage: itemBgImg },
  { id: 2, name: "PS5", stock: 2, weight: 2, theme: "blue", resultImage: itemBgImg },
  { id: 3, name: "iPad", stock: 3, weight: 5, theme: "blue", resultImage: itemBgImg },
  { id: 4, name: "HyperX", stock: 5, weight: 10, theme: "green", resultImage: itemBgImg },
  { id: 5, name: 'Monitor 27"', stock: 3, weight: 5, theme: "blue", resultImage: itemBgImg },
  { id: 6, name: "Keyboard", stock: 10, weight: 15, theme: "gray", resultImage: itemBgImg },
  { id: 7, name: "Mouse", stock: 10, weight: 15, theme: "gray", resultImage: itemBgImg },
  { id: 8, name: "Gacha x100", stock: 100, weight: 50, theme: "gray", resultImage: itemBgImg },
]);

// Word Data
const targetWord = reactive([
  { char: "A", image: letterA, collected: false },
  { char: "N", image: letterN, collected: false },
  { char: "N", image: letterN, collected: false },
  { char: "I", image: letterI, collected: false },
  { char: "V", image: letterV, collected: false },
  { char: "E", image: letterE, collected: false },
  { char: "R", image: letterR, collected: false },
  { char: "S", image: letterS, collected: false },
  { char: "A", image: letterA, collected: false },
  { char: "R", image: letterR, collected: false },
  { char: "Y", image: letterY, collected: false },
]);

const getSegmentStyle = (theme) => {
  let color = '156, 163, 175'; // Default Gray

  if (theme === 'red') color = '220, 38, 38';
  else if (theme === 'blue') color = '32, 156, 233';
  else if (theme === 'green') color = '34, 197, 94';

  return {
    background: `
      linear-gradient(0deg, rgba(${color}, 0.2), rgba(${color}, 0.2)),
      linear-gradient(186.63deg, #2D2D2D 6.53%, #2C2C2C 96.3%)
    `,
    border: '2px solid transparent',
    borderImageSource: 'linear-gradient(186.7deg, rgba(255, 255, 255, 0.5) 5.26%, rgba(255, 255, 255, 0) 63.54%)',
    borderImageSlice: '1'
  };
};

// --- Function Logic ---

const isAllCollected = computed(() => {
  return targetWord.every((item) => item.collected === true);
});

// Click word slot
const handleSlotClick = (index) => {
  if (remainingSpins.value <= 0) return;

  if (!targetWord[index].collected) {
    targetWord[index].collected = true;
  }
};

// State spin wheel
const showWheelModal = ref(false);
const showResultModal = ref(false);
const isSpinning = ref(false);
const wheelRotation = ref(0);
const wonReward = ref(null);

// Modal spinwheel
const openWheelModal = () => {
  if (remainingSpins.value > 0) {
    showWheelModal.value = true;
  } else {
    alert("สิทธิ์หมุนของคุณหมดแล้ว!");
  }
};

// Spin Logic
const spinTheWheel = () => {
  if (remainingSpins.value <= 0 || isSpinning.value) return;

  remainingSpins.value--;
  isSpinning.value = true;

  // Filter stock > 0
  const availableRewards = rewards.value.filter((r) => r.stock > 0);

  if (availableRewards.length === 0) {
    alert("ของรางวัลหมดเกลี้ยงแล้วครับ!");
    isSpinning.value = false;
    return;
  }

  // Weighted random selection
  const totalWeight = availableRewards.reduce(
    (sum, item) => sum + item.weight,
    0
  );
  let randomNum = Math.random() * totalWeight;
  let selectedReward = null;

  for (const item of availableRewards) {
    if (randomNum < item.weight) {
      selectedReward = item;
      break;
    }
    randomNum -= item.weight;
  }

  if (!selectedReward) selectedReward = availableRewards[0];

  const rewardIndex = rewards.value.findIndex(
    (r) => r.id === selectedReward.id
  );
  const segmentAngle = 360 / rewards.value.length;

  const totalDegree = 5400 * spinCount.value;

  const stopAngle = totalDegree - (rewardIndex * segmentAngle) - (segmentAngle / 2);
  const randomOffset = Math.floor(Math.random() * 20) - 10;

  wheelRotation.value = stopAngle + randomOffset;

  spinCount.value++;

  setTimeout(() => {
    isSpinning.value = false;
    wonReward.value = selectedReward;

    // Decrease stock
    const realRewardIndex = rewards.value.findIndex(
      (r) => r.id === selectedReward.id
    );
    if (realRewardIndex !== -1) {
      rewards.value[realRewardIndex].stock--;
    }

    showResultModal.value = true;
  }, 3000);
};

// Reset
const resetGameRound = () => {
  showResultModal.value = false;
  showWheelModal.value = false;
  targetWord.forEach((item) => (item.collected = false));
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Titan+One&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&display=swap");
@import url('https://fonts.googleapis.com/css2?family=Allison&display=swap');

.font-line-seed {
  font-family: 'LINE Seed Sans TH', sans-serif;
}

.font-lucky-wheel {
  font-family: 'Allison', cursive;
  font-weight: 400;
  font-style: normal;
  font-size: 64px;
  line-height: 100%;
  letter-spacing: 0%;
}

.font-balloon {
  font-family: "Titan One", cursive;
  -webkit-text-stroke: 1.5px #ffffff;
  filter: drop-shadow(2px 3px 4px rgba(0, 0, 0, 0.4));
}

.font-script {
  font-family: "Dancing Script", cursive;
}

.text-balloon-blue {
  color: #60a5fa;
}

.text-balloon-purple {
  color: #a855f7;
}

.text-balloon-cyan {
  color: #22d3ee;
}

.sunburst {
  background: repeating-conic-gradient(from 0deg,
      rgba(0, 0, 0, 0.8) 0deg 10deg,
      rgba(30, 58, 138, 0.2) 10deg 20deg);
}

.sunburst-ray {
  background: repeating-conic-gradient(from 0deg,
      rgba(255, 255, 255, 0.05) 0deg 15deg,
      transparent 15deg 30deg);
}

.bg-radial-gradient {
  background-image: radial-gradient(circle,
      var(--tw-gradient-from) 0%,
      var(--tw-gradient-via) 40%,
      var(--tw-gradient-to) 100%);
}

.bg-radial-gradient-center {
  background-image: radial-gradient(circle,
      transparent 0%,
      rgba(0, 0, 0, 0.4) 60%,
      rgba(0, 0, 0, 0.9) 100%);
}

@keyframes spin-slow {
  from {
    transform: rotate(0deg) scale(1.5);
  }

  to {
    transform: rotate(360deg) scale(1.5);
  }
}


@keyframes pulse-slow {

  0%,
  100% {
    opacity: 1;
    transform: scale(1);
  }

  50% {
    opacity: 0.8;
    transform: scale(1.02);
  }
}

.animate-pulse-slow {
  animation: pulse-slow 3s infinite;
}


.ease-spin-fast {
  transition-timing-function: cubic-bezier(0.2, 0.8, 0.3, 1);
}

.animate-fade-in {
  animation: fadeIn 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.95);
  }

  to {
    opacity: 1;
    transform: scale(1);
  }
}
</style>