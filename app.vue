<template>
    <div>
        <div>{{ duration }}</div>
        <div v-for="list in lists" :key="list">
            <button @click="playSound(list)">Play</button>
            <button @click="stopSound">Stop</button>
            <button @click="pauseSound">Pause</button>
        </div>
        <button @click="getCurrentTime">Current Type</button>
    </div>
</template>
<script setup lang="ts">
let htmlAudio: any = ref();
const isPlaying = ref(false);
const currentTime = ref(0);
const duration = ref(0);

const lists = [
    "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3",
    // "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3",
];

const playSound = async (music: string) => {
    if (isPlaying.value) {
        htmlAudio.pause();
    }
    htmlAudio = await new Audio(music);
    duration.value = htmlAudio.duration;
    htmlAudio.currentTime = currentTime.value;
    htmlAudio.play();
    isPlaying.value = true;
};

const stopSound = () => {
    htmlAudio.pause();
    currentTime.value = 0;
    isPlaying.value = false;
};

const pauseSound = () => {
    currentTime.value = htmlAudio.currentTime;
    htmlAudio.pause();
};
</script>