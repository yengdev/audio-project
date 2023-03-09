<template>
  <div>
    <div class="text-xl">Discover</div>
    <div class="mt-3">
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div class="col-span-2 order-2 md:order-1">
          <div class="font-bold">Trading podcasts</div>
          <div class="mt-1">
            <div
              v-for="(list, index) in playList"
              :key="index"
              :class="`${
                selectedPlayList === list
                  ? 'bg-indigo-500 text-slate-50'
                  : 'bg-white'
              }  p-3 rounded-lg mb-3 cursor-pointer`"
              @click="selectedPlayList = list"
            >
              <div
                :class="`text-sm  ${
                  selectedPlayList === list
                    ? 'text-slate-100'
                    : 'text-slate-500'
                }`"
              >
                Podcast
              </div>
              <div class="font-bold">How to earn money from this Website</div>
              <div
                :class="`text-sm mt-1 ${
                  selectedPlayList === list
                    ? 'text-slate-100'
                    : 'text-slate-500'
                }`"
              >
                By Kayengxiong TST
              </div>
            </div>
          </div>
        </div>
        <div class="order-1 md:order-2">
          <div class="bg-white w-full p-5 rounded-lg">
            <div>How to earn money</div>
            <div class="text-sm text-slate-500">by Kayengxiong TST</div>
            <div class="font-light text-slate-500 mt-3">
              Lorem ipsum dolor sit amet, consectetur adipisicing elit. Eius
              ipsam beatae ullam. Tempora, dolores porro rem maxime enim
              possimus, repellat error quibusdam qui expedita eligendi itaque
              accusamus cum! Qui, recusandae.
            </div>
            <div>
              <input
                v-model="runningTime"
                type="range"
                :max="duration"
                class="w-full h-2 bg-indigo-700 rounded-lg appearance-none cursor-pointer dark:bg-indigo-700"
                @change="onChangeSlider"
              />
            </div>
            <div class="flex justify-between">
              <div class="text-sm">{{ getMinSec() }}</div>
              <div class="text-sm">{{ formatDateTime(duration) }}</div>
            </div>
            <div class="text-center">
              <button
                v-if="!isPlaying"
                class="bg-indigo-700 p-2 rounded-full"
                @click="playSound()"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-6 h-6 text-indigo-100"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.348a1.125 1.125 0 010 1.971l-11.54 6.347a1.125 1.125 0 01-1.667-.985V5.653z"
                  />
                </svg>
              </button>
              <button
                v-else
                class="bg-indigo-700 p-2 rounded-full"
                @click="pauseSound"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-6 h-6 text-indigo-100"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M15.75 5.25v13.5m-7.5-13.5v13.5"
                  />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
let htmlAudio: any = ref();
const currentTime = ref(0);
const isPlaying = ref(false);
const duration = ref(0);
const runningTime = ref(0);
const selectedPlayList = ref();
const isLoading = ref(false);

let playingInterval: any = null;

const playList = [
  "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3",
  "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3",
  "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3",
];

const playSound = async () => {
  isLoading.value = true;
  if (isPlaying.value) {
    htmlAudio.pause();
  }
  htmlAudio = await new Audio(selectedPlayList.value);
  htmlAudio.onloadedmetadata = () => {
    isLoading.value = false;
    duration.value = htmlAudio.duration;
    htmlAudio.currentTime = currentTime.value;
    htmlAudio.play();
    isPlaying.value = true;
    playingInterval = setInterval(() => {
      runningTime.value = ~~runningTime.value + 1;
    }, 1000);
  };
};

const pauseSound = () => {
  currentTime.value = htmlAudio.currentTime;
  pause();
};

const pause = () => {
  isPlaying.value = false;
  clearInterval(playingInterval);
  htmlAudio.pause();
};

function str_pad_left(str: any, pad: string, length: number) {
  return (new Array(length + 1).join(pad) + str).slice(-length);
}

const getMinSec = () => {
  let time = formatDateTime(runningTime.value);
  return time;
};
const formatDateTime = (second: number) => {
  let time = ~~second;
  const minutes = Math.floor(time / 60);
  const seconds = time - minutes * 60;
  const hours = Math.floor(time / 3600);
  time = time - hours * 3600;

  const finalTime =
    str_pad_left(minutes, "0", 2) + ":" + str_pad_left(seconds, "0", 2);
  return finalTime;
};

const onChangeSlider = (e: any) => {
  pause();
  currentTime.value = ~~e.target.value;
  htmlAudio.currentTime = currentTime.value;
  htmlAudio.play();
  isPlaying.value = true;
  playingInterval = setInterval(() => {
    runningTime.value = ~~runningTime.value + 1;
  }, 1000);
};
watch(runningTime, (value) => {
  if (value >= duration.value) {
    clearInterval(playingInterval);
  }
});
</script>
