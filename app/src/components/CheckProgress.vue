<template>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron&display=swap" rel="stylesheet">
    <div class="h-[24%] bg-[rgb(0,0,0)] rounded-2xl flex justify-center items-center">
        <h1 class="text-white font-bold text-6xl " style="font-family: 'Orbitron', sans-serif; letter-spacing: 0.1em;"> {{currentTime}}</h1>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const currentTime = ref('')

function updateTime() {
  const now = new Date()
  currentTime.value = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' }) // Formats time as "HH:MM AM/PM"
}

onMounted(() => {
  updateTime()
  setInterval(updateTime, 1000) // Update every second if you want a live clock
})
</script>



<style lang="scss" scoped>

</style>