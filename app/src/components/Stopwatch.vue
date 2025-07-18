<template>
    <div class="w-full h-full flex flex-col items-center">
      <h1>{{ convert(currentTime) }}</h1>
      <button v-if="!showpaused" class="w-[80%] bg-amber-500 rounded-4xl h-[10%]" @click="start">
        <i class="fa-solid fa-play"></i>
      </button>
      <button v-if="showpaused" class="w-[80%] bg-amber-500 rounded-4xl h-[10%]" @click="stop">
        <i class="fa-solid fa-pause"></i>
      </button>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const showpaused = ref(false);
  const currentTime = ref(0);
  let timerId = null;
  function convert(timeinsec) {
  const hours = Math.floor(timeinsec / 3600);
  const min = Math.floor((timeinsec % 3600) / 60);
  const sec = timeinsec % 60;

  // Seconds and minutes padding only if hours or minutes > 0
  const mm = (hours > 0) ? min.toString().padStart(2, '0') : min.toString();
  const ss = (hours > 0 || min > 0) ? sec.toString().padStart(2, '0') : sec.toString();

  if (hours > 0) {
    return `${hours}:${mm}:${ss}`;
  } else if (min > 0) {
    return `${min}:${ss}`;
  } else {
    return `${sec}`;
  }
}



  function start() {
    if (timerId) return; // Prevent multiple intervals
  
    showpaused.value = true;
    timerId = setInterval(() => {
      currentTime.value += 1;
    }, 1000);
  }
  
  function stop() {
    showpaused.value = false;
    clearInterval(timerId);
    timerId = null;
  }
  </script>
  
  <style scoped>
  </style>
  