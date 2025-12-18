<template>
  <div
    class="relative w-full h-screen bg-gray-900 overflow-hidden text-white font-sans selection:bg-blue-500 selection:text-white">
    <img :src="bgImage" alt="Background" class="absolute inset-0 w-full h-full object-cover z-0" />

    <div class="absolute inset-0 sunburst opacity-60 animate-spin-slow z-1"></div>
    <div class="absolute inset-0 bg-radial-gradient from-transparent via-black/60 to-black z-1"></div>

    <div class="absolute bottom-2 left-2 md:bottom-4 md:left-4 z-[100] opacity-90 pointer-events-none">
      <img :src="hudImg" alt="HUD" class="h-10 md:h-14 w-auto drop-shadow-lg" />
    </div>

    <div
      class="absolute top-4 right-4 md:top-6 md:right-6 z-[100] flex items-center gap-3 bg-black rounded-full px-4 py-2 md:px-6 md:py-3 border border-gray-800 shadow-xl">
      <div class="w-6 h-6 md:w-8 md:h-8 flex items-center justify-center">
        <img src="../assets/img/Lucky_wheel.png" alt="Icon"
          class="w-full h-full object-contain filter drop-shadow-md" />
      </div>
      <span class="text-white font-bold text-sm md:text-xl tracking-wide font-sans">
        Remaining spin : {{ remainingSpins }}
      </span>
    </div>

    <div class="relative z-10 w-full h-full flex flex-col">
      <div class="flex-1 flex flex-col items-center justify-center px-4 transition-all duration-500"
        :class="showWheelModal ? 'blur-sm scale-95 opacity-50' : ''">
        <div class="text-center space-y-4 mb-8">
          <h1
            class="text-5xl md:text-7xl font-display font-black tracking-tighter text-white drop-shadow-[0_0_15px_rgba(255,255,255,0.5)] flex justify-center items-center">
            <img src="../assets/img/Logo.png" alt="Logo" class="max-h-24 md:max-h-32" />
          </h1>
          <p class="text-blue-300/80 text-sm md:text-base italic max-w-2xl mx-auto">
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolorem
            iure explicabo fuga.
          </p>
        </div>

        <div class="flex flex-wrap justify-center gap-2 md:gap-3 py-8 max-w-5xl">
          <div v-for="(item, index) in targetWord" :key="index" class="group relative">
            <button @click="handleSlotClick(index)" :disabled="item.collected || remainingSpins <= 0"
              class="w-12 h-16 md:w-20 md:h-24 flex items-center justify-center rounded-lg border-2 transition-all duration-300 transform"
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
            class="px-12 py-3 rounded-full font-bold text-xl tracking-wider transition-all duration-300 transform border-2"
            :class="[
              isAllCollected
                ? 'bg-gradient-to-r from-blue-600 to-cyan-500 hover:from-blue-500 hover:to-cyan-400 border-white/20 text-white shadow-[0_0_30px_rgba(6,182,212,0.6)] hover:scale-105 active:scale-95 cursor-pointer animate-pulse-slow'
                : 'bg-gray-800 text-gray-500 border-gray-700 cursor-not-allowed opacity-60',
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

        <div class="relative w-[380px] h-[380px] md:w-[600px] md:h-[600px] flex items-center justify-center">
          <div class="absolute -top-6 left-1/2 -translate-x-1/2 z-30 w-16 h-20 md:w-24 md:h-28 filter drop-shadow-xl">
            <img :src="pinImg" class="w-full h-full object-contain" alt="Pin" />
          </div>

          <div class="absolute inset-0 z-0 flex items-center justify-center pointer-events-none scale-105">
            <img :src="wrapperImg" class="w-full h-full object-contain opacity-90 drop-shadow-2xl" alt="Wrapper" />
          </div>

          <div
            class="relative z-10 w-full h-full rounded-full border-[8px] md:border-[12px] border-[#3b82f6] shadow-[0_0_50px_rgba(37,99,235,0.5)] overflow-hidden transition-transform duration-[3000ms] ease-spin-fast"
            :style="{ transform: `rotate(${wheelRotation}deg)` }">
            <div class="absolute inset-0 bg-[#0f172a]"></div>

            <div v-for="(reward, index) in rewards" :key="'bg-' + reward.id"
              class="absolute top-0 left-1/2 w-1/2 h-[50%] origin-bottom-left border-l border-white/10" :style="{
                transform: `rotate(${index * 45}deg) skewY(-45deg)`,
                background: index % 2 === 0 ? '#1e293b' : '#334155',
              }"></div>

            <div class="absolute inset-0 w-full h-full pointer-events-none">
              <div v-for="(reward, index) in rewards" :key="'content-' + reward.id"
                class="absolute top-1/2 left-1/2 w-0 h-0 flex justify-center items-center" :style="{
                  transform: `rotate(${index * 45 + 22.5}deg)`,
                }">
                <div
                  class="flex flex-col items-center justify-center text-center transform -translate-y-[110px] md:-translate-y-[180px] w-32 md:w-40">
                  <img :src="reward.resultImage" alt="item"
                    class="w-16 h-auto md:w-28 object-contain mb-2 drop-shadow-[0_4px_4px_rgba(0,0,0,0.5)] transform hover:scale-110 transition-transform" />

                  <div
                    class="text-center bg-black/30 backdrop-blur-[2px] rounded-lg px-2 py-1 w-full flex flex-col items-center border border-white/5">
                    <span
                      class="block text-[10px] md:text-sm font-bold text-white leading-tight shadow-black drop-shadow-md break-words max-w-full">
                      {{ reward.name }}
                    </span>
                    <span class="block text-[9px] md:text-[10px] text-cyan-300 mt-0.5 font-medium"
                      v-if="reward.stock > 0">
                      x{{ reward.stock }}
                    </span>
                    <span class="block text-[9px] text-red-400 mt-0.5" v-else>
                      หมด
                    </span>
                  </div>
                </div>
              </div>
            </div>

            <div
              class="absolute inset-0 m-auto w-[30%] h-[30%] rounded-full bg-gradient-to-b from-[#1e293b] to-[#0f172a] border-[4px] md:border-[6px] border-[#3b82f6] shadow-[inset_0_0_20px_rgba(0,0,0,0.8)] z-20 flex items-center justify-center">
              <div
                class="w-full h-full rounded-full bg-blue-500/10 flex items-center justify-center animate-pulse-slow">
                <span class="font-display font-black text-blue-400/30 text-2xl md:text-4xl tracking-widest">LUV</span>
              </div>
            </div>
          </div>

          <button @click="spinTheWheel" :disabled="isSpinning || remainingSpins <= 0"
            class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 z-30 w-20 h-20 md:w-28 md:h-28 bg-gradient-to-b from-[#38bdf8] to-[#0284c7] rounded-full flex items-center justify-center shadow-[0_0_30px_rgba(56,189,248,0.6)] border-[4px] md:border-[6px] border-[#0f172a] hover:scale-110 active:scale-95 transition-all group overflow-hidden"
            :class="remainingSpins <= 0 ? 'grayscale cursor-not-allowed' : ''">
            <div
              class="absolute inset-0 bg-white/20 translate-y-full group-hover:translate-y-0 transition-transform duration-300">
            </div>
            <span
              class="relative font-black text-xl md:text-3xl italic tracking-tighter text-white drop-shadow-md group-hover:animate-pulse">
              SPIN
            </span>
          </button>
        </div>

        <button v-if="!isSpinning" @click="showWheelModal = false"
          class="mt-4 text-gray-500 text-sm underline hover:text-white transition-colors">
          กลับไปหน้าหลัก
        </button>
      </div>
    </div>

    <div v-if="showResultModal"
      class="fixed inset-0 z-[60] flex items-center justify-center bg-black animate-fade-in overflow-hidden">
      <div class="absolute inset-0 transition-colors duration-1000 opacity-60" :class="[
        wonReward?.theme === 'red'
          ? 'bg-red-900'
          : wonReward?.theme === 'green'
            ? 'bg-green-900'
            : wonReward?.theme === 'blue'
              ? 'bg-blue-900'
              : 'bg-gray-800',
      ]"></div>

      <div class="absolute inset-0 opacity-40 sunburst-ray animate-spin-slow z-1"></div>
      <div class="absolute inset-0 bg-radial-gradient-center z-1"></div>

      <div class="relative z-10 flex flex-col items-center justify-center text-center w-full max-w-4xl px-4">
        <div class="relative mb-8">
          <h2 class="text-4xl md:text-6xl font-bold text-white tracking-widest drop-shadow-lg uppercase">
            CONGRATULATIONS!
          </h2>
          <p class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 font-script text-6xl md:text-6xl opacity-90 whitespace-nowrap mt-6"
            :class="[
              wonReward?.theme === 'red'
                ? 'text-red-500'
                : wonReward?.theme === 'green'
                  ? 'text-green-400'
                  : wonReward?.theme === 'blue'
                    ? 'text-blue-400'
                    : 'text-gray-300',
            ]" style="text-shadow: 0 2px 4px rgba(0, 0, 0, 0.8)">
            Lucky Wheel
          </p>
        </div>

        <p class="text-gray-200 text-lg md:text-xl font-light mb-10">
          Wow! You've just won a
          <span class="font-bold text-white">{{ wonReward?.name }}</span>
        </p>

        <div class="relative flex items-center justify-center w-full py-10 mb-8">
          <div class="absolute w-[400px] h-[400px] rounded-full border-2 border-white/10 scale-100 animate-pulse-slow">
          </div>
          <div class="absolute w-[300px] h-[300px] rounded-full border border-white/20 scale-90"></div>

          <div class="absolute w-64 h-64 rounded-full blur-[80px]" :class="[
            wonReward?.theme === 'red'
              ? 'bg-red-600/60'
              : wonReward?.theme === 'green'
                ? 'bg-green-500/60'
                : wonReward?.theme === 'blue'
                  ? 'bg-blue-500/60'
                  : 'bg-white/30',
          ]"></div>

          <img :src="wonReward?.resultImage"
            class="relative w-64 md:w-80 h-auto object-contain drop-shadow-[0_10px_30px_rgba(0,0,0,0.8)] transform hover:scale-105 transition-transform duration-500"
            alt="Reward" />
        </div>

        <button @click="resetGameRound"
          class="px-10 py-3 bg-white text-black font-bold text-lg rounded-full shadow-[0_0_20px_rgba(255,255,255,0.4)] hover:bg-gray-100 hover:scale-105 transition-all duration-300">
          Spin Again
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from "vue";

// --- Import Asset ---
import pinImg from "../assets/img/Pin.png";
import wrapperImg from "../assets/img/wrapper.png";
import imgRed from "../assets/img/image-red.png";
import imgBlue from "../assets/img/image-blue.png";
import imgGreen from "../assets/img/image-green.png";
import imgGray from "../assets/img/image-gray.png";
import hudImg from "../assets/img/HeartBar.png";
import bgImage from "../assets/img/Drop-frame.png";
import letterA from "../assets/img/A.png";
import letterN from "../assets/img/N.png";
import letterI from "../assets/img/I.png";
import letterV from "../assets/img/V.png";
import letterE from "../assets/img/E.png";
import letterR from "../assets/img/R.png";
import letterS from "../assets/img/S.png";
import letterY from "../assets/img/Y.png";

// --- Configuration ---
const remainingSpins = ref(3); 
const spinCount = ref(1); // [เพิ่ม] ตัวนับรอบการหมุน เพื่อให้องศามันเพิ่มขึ้นเรื่อยๆ

// Reward Data
const rewards = ref([
  { id: 1, name: "iPhone 17", stock: 1, weight: 1, theme: "red", resultImage: imgRed },
  { id: 2, name: "PS5", stock: 2, weight: 2, theme: "blue", resultImage: imgBlue },
  { id: 3, name: "iPad", stock: 3, weight: 5, theme: "blue", resultImage: imgBlue },
  { id: 4, name: "HyperX", stock: 5, weight: 10, theme: "green", resultImage: imgGreen },
  { id: 5, name: 'Monitor 27"', stock: 3, weight: 5, theme: "blue", resultImage: imgBlue },
  { id: 6, name: "Keyboard", stock: 10, weight: 15, theme: "gray", resultImage: imgGray },
  { id: 7, name: "Mouse", stock: 10, weight: 15, theme: "gray", resultImage: imgGray },
  { id: 8, name: "Gacha x100", stock: 100, weight: 50, theme: "gray", resultImage: imgGray },
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

.animate-spin-slow {
  animation: spin-slow 60s linear infinite;
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

/* FIX: Custom Easing for Fast Spin Effect */

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
