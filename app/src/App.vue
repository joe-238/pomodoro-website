<template>
  <div class= "w-full h-screen bg-white flex justify-center gap-3 items-center">
    <div class="w-1/3 h-[80%]  flex flex-col items-center rounded-2xl bg-[rgba(0,0,0,0.2)] pt-[2%] bg-[url('/studybg.jpg')] bg-cover bg-center">
      <div class="w-[80%] h-[10%] bg-blue-400 rounded-4xl flex justify-between ">
        <button @click="container = 'timer'" class="w-[47%] font-bold h-full bg-red-600 rounded-4xl">timer</button>
        <button @click="container = 'stopwatch'" class="w-[47%] font-bold h-full  rounded-4xl">stopwatch</button>
      </div>
        <Stopwatch v-if="container === 'stopwatch'" />
        <Timer v-if="container === 'timer'" />
    </div>
    <div class="flex flex-col w-1/3 h-[80%]  justify-between">
      <CheckProgress/>
      <ToDoList/>
    </div>
  </div>
</template>

<script setup>
import Stopwatch from './components/Stopwatch.vue';
import ToDoList from './components/ToDoList.vue';
import Timer from './components/Timer.vue';
import CheckProgress from './components/CheckProgress.vue';
import { ref } from 'vue';

const container = ref('timer')
</script>

<style lang="scss" scoped>

</style>