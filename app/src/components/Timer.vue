<template>
    <div class="w-full h-full flex flex-col items-center justify-evenly">
      <div class="w-full flex flex-col items-center">
        <input
          class="w-[80%] placeholder:text-center focus:outline-none text-center font-[digital-clock-font] font-stretch-90% text-[rgba(255,255,255,0.7)] font-bold text-7xl "
          :value="isFocused ? usertime : currenttime"
          @input="e => usertime = e.target.value"
          @focus="isFocused = true"
          @blur="handleBlur"
          placeholder="00:00:00"
        />
        <h1 class="text-white font-medium">pomodoro.</h1>
      </div>
      <div class="w-[50%] h-[10%] flex items-center justify-between">
        <button v-if="!showpaused" class="w-[80%] bg-amber-500 text-black rounded-4xl h-full" @click="start"><i class="fa-solid fa-play"></i></button>
        <button v-if="showpaused" class="w-[80%] bg-amber-500 rounded-4xl h-full" @click="stop"><i class="fa-solid fa-pause"></i></button>
        <i class="fa-solid fa-rotate-right text-white text-4xl"></i>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  const showpaused = ref(false)
  const usertime = ref('');
  const currenttime = ref('');
  const isFocused = ref(false);
  let countdown = 0; // seconds remaining
  let timerId = null;
  
  function onBlur() {
    const newVal = usertime.value;
    if (!newVal) {
      currenttime.value = '';
      countdown = 0;
      return;
    }
  
    const time = [];
    for (let i = newVal.length; i > 0; i -= 2) {
      time.unshift(newVal.slice(Math.max(i - 2, 0), i));
    }
  
    if (time.length === 1) {
      time.unshift("0");
    }
  
    for (let i = time.length - 1; i > 0; i--) {
      let num = parseInt(time[i], 10); //make to num in tens 
      if (num >= 60) {
        const carry = Math.floor(num / 60);
        const remainder = num % 60;
        time[i] = remainder.toString().padStart(2, '0');
        time[i - 1] = (parseInt(time[i - 1], 10) + carry).toString();
      }
    }
  
    if (time.length > 0) {
      let hours = parseInt(time[0], 10);
      if (hours >= 60 && time.length === 3) {
        time[0] = hours.toString();
      } else if (hours >= 60 && time.length < 3) {
        const carry = Math.floor(hours / 60);
        const remainder = hours % 60;
        time[0] = remainder.toString().padStart(2, '0');
        time.unshift(carry.toString());
      }
    }
  
    const padded = time.map((part, index) =>
      index === 0 ? part : part.toString().padStart(2, '0')
    );
  
    currenttime.value = padded.join(':');
    console.log('Converted on blur:', currenttime.value);
  
    countdown = timeToSeconds(padded);
  }
  
  function handleBlur(e) {
    isFocused.value = false;
    onBlur();
  }
  
  function timeToSeconds(parts) {
    let seconds = 0;
    if (parts.length === 3) {
      seconds += parseInt(parts[0], 10) * 3600;
      seconds += parseInt(parts[1], 10) * 60;
      seconds += parseInt(parts[2], 10);
    } else if (parts.length === 2) {
      seconds += parseInt(parts[0], 10) * 60;
      seconds += parseInt(parts[1], 10);
    } else if (parts.length === 1) {
      seconds += parseInt(parts[0], 10);
    }
    return seconds;
  }
  
  function secondsToTime(sec) {
    const h = Math.floor(sec / 3600);
    const m = Math.floor((sec % 3600) / 60);
    const s = sec % 60;
    return [h, m, s];
  }
  
  function formatCountdown(sec) {
    if (sec <= 0) return "0:00:00";

    const [h, m, s] = secondsToTime(sec);

    if (h > 0) {
        // Show H:MM:SS with padded minutes and seconds
        return `${h}:${m.toString().padStart(2, '0')}:${s.toString().padStart(2, '0')}`;
    } else if (m > 0) {
        // Show M:SS with seconds padded
        return `${m}:${s.toString().padStart(2, '0')}`;
    } else {
        // Show seconds only, no padding
        return `${s}`;
    }
    }

  
  function start() {
    if (timerId || countdown <= 0) return;
  
    timerId = setInterval(() => {
      if (countdown > 0) {
        countdown--;
        currenttime.value = formatCountdown(countdown);
      } else {
        stop();
      }
    }, 1000);
    showpaused.value = true
  }
  
  function stop() {
    clearInterval(timerId);
    timerId = null;
    showpaused.value = false
  }
  </script>
  