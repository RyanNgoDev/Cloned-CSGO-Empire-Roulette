<template>
  <div class="mx-auto w-full pb-xl">
    <div id="wheel" :style="imgPosition" class="coins relative mx-auto mb-8 w-full">
      <div class="marker absolute z-10"></div>
    </div>
    <div class="w-full text-center">
      <input
        :disabled="isRolling"
        class="w-1/2 p-2 bg-slate-800 disabled:bg-slate-900 disabled:text-gray-700"
        v-model="wager"
      />
    </div>
    <div class="mx-auto w-1/2 p-4 flex gap-4 justify-between">
      <button
        :disabled="isRolling"
        class="rounded text-2xl bg-amber-400 py-4 w-1/4 text-neutral-400 disabled:bg-amber-500 disabled:text-neutral-500"
        @click="changeValue('T')"
      >
        T
      </button>
      <button
        :disabled="isRolling"
        class="rounded text-2xl bg-slate-50 py-4 w-1/4 text-neutral-400 disabled:bg-slate-200 disabled:text-neutral-500"
        @click="changeValue('Dice')"
      >
        Dice
      </button>
      <button
        :disabled="isRolling"
        class="rounded text-2xl bg-slate-600 py-4 w-1/4 text-neutral-400 disabled:bg-slate-700 disabled:text-neutral-500"
        @click="changeValue('CT')"
      >
        CT
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import soundEffect from "../assets/audio/cashin.mp3";
const coins = useState<number>("coins");
let wager = 0;
const position = ref(570);
let isRolling = false;

const imgPosition = computed(() => ({
  backgroundPosition: position.value + "px center",
  transition: "none 0s ease 0s",
}));
const changeValue = async (selection: string) => {
  if (isRolling) return;

  if (coins.value < wager) {
    window.alert("Insufficient bet amount");
    return;
  }

  isRolling = true;
  coins.value -= wager;
  position.value = 6200;
  const randomNumber = generateRandomResult();
  const res = calculateResult(randomNumber);
  const offset = document.getElementById("wheel")!.offsetWidth * 0.5;
  const newPosition = offset + 1500 - randomNumber;
  decreasePosition(newPosition, () => {
    calculateTicket(selection, res);
    isRolling = false;
  });
};

function calculateTicket(selection: string, result: string) {
  if (selection !== result) {
    return;
  } else {
    const audio = new Audio(soundEffect);
    audio.play();
    if (selection === "Dice")
      coins.value = parseInt(coins.value.toString()) + 14 * parseInt(wager.toString());
    else coins.value = parseInt(coins.value.toString()) + 2 * parseInt(wager.toString());
  }
}

function generateRandomResult() {
  let randomNumber;
  do {
    randomNumber = Math.floor(Math.random() * 1500);
  } while (randomNumber % 10 === 0 || randomNumber % 10 === 1 || randomNumber % 10 === 9);

  return randomNumber;
}

function calculateResult(value: number) {
  if (Math.floor(value / 100) === 14) return "Dice";

  if (Math.floor(value / 100) % 2 === 0) return "CT";

  return "T";
}

function decreasePosition(finalVal: number, callback: () => void) {
  const startTime = performance.now();
  const duration = 4000;
  let remainingTime = duration;

  const intervalId = setInterval(() => {
    const elapsedTime = performance.now() - startTime;
    remainingTime = duration - elapsedTime;

    const slowdownFactor = Math.pow(remainingTime / duration, 4);

    position.value = finalVal + 6200 * slowdownFactor;

    if (remainingTime <= 0) {
      clearInterval(intervalId);
      callback();
    }
  }, 10);
}
</script>

<style scoped>
.coins {
  max-width: 1100px;
  height: 100px;
  background-image: url(https://csgoempire.com/assets/coins-V2e_1E6W.png);
  background-size: auto 100px;
  background-repeat: repeat-x;
  transform: translateZ(0);
}

.coins:before {
  left: 0;
  background: linear-gradient(90deg, rgba(26, 28, 36, 0.9) 0%, transparent 100%);
}

.coins:after {
  right: 0;
  background: linear-gradient(270deg, rgba(26, 28, 36, 0.9) 0%, transparent 100%);
}

.coins:before,
.coins:after {
  content: "";
  position: absolute;
  top: 0;
  width: 25%;
  height: 100%;
}

.marker {
  top: -10px;
  bottom: -10px;
  left: 50%;
  transform: translate(-50%);
  width: 4px;
  background: #d0021b;
  border-radius: 4px;
}
</style>
