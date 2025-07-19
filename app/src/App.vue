<template>
  <div class="w-full h-screen bg-white flex justify-center gap-3 items-center">
    <!-- TIMER / STOPWATCH SECTION -->
    <div class="w-1/3 h-[80%] flex flex-col items-center rounded-2xl  pt-[2%] bg-[url(/backgroundpicyuh.png)] bg-cover bg-center overflow-hidden">

      <!-- Toggle Buttons -->
      <div class="w-[80%] h-[10%] bg-[#3C3C3C] border-5 border-[#3C3C3C] rounded-4xl relative flex justify-between items-center overflow-hidden">
        <!-- Sliding Indicator -->
        <div
          class="absolute h-full w-[47%] bg-white z-0 rounded-4xl transition-all duration-300"
          :class="container === 'timer' ? 'left-0' : 'left-[53%]'"
        ></div>
        <!-- Buttons -->
        <button
          @click="container = 'timer'"
          class="w-[47%] h-full font-semibold z-10 transition-all duration-300"
          :class="[container === 'timer' ? 'text-black' : 'text-white']"
        >
          Timer
        </button>

        <button
          @click="container = 'stopwatch'"
          class="w-[47%] h-full font-semibold z-10 transition-all duration-300"
          :class="[container === 'stopwatch' ? 'text-black' : 'text-white']"
        >
          Stopwatch
        </button>

      </div>

      <!-- Sliding Components -->
      <div class="relative w-full h-[90%] overflow-hidden">
        <div
          class="flex w-[200%] h-full transition-transform duration-500"
          :style="{ transform: container === 'timer' ? 'translateX(0%)' : 'translateX(-50%)' }"
        >
          <div class="w-1/2 h-full">
            <Timer />
          </div>
          <div class="w-1/2 h-full">
            <Stopwatch />
          </div>
        </div>
      </div>
      
    </div>

    <!-- CHECK PROGRESS / TODO LIST SECTION -->
    <div class="flex flex-col w-1/3 h-[80%] justify-between">
      <CheckProgress />
      <ToDoList />
    </div>
  </div>
</template>


<script setup>
import Stopwatch from './components/Stopwatch.vue';
import Timer from './components/Timer.vue';
import ToDoList from './components/ToDoList.vue';
import CheckProgress from './components/CheckProgress.vue';
import { ref } from 'vue';

const container = ref('timer');
</script>


<style lang="scss" scoped>

</style>