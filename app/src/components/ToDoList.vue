<template>
  <div
    class="h-[74%] border-4 bg-[#3c3c3cde] bg-clip-padding backdrop-filter backdrop-blur-sm bg-opacity-40 backdrop-saturate-150 backdrop-contrast-125 rounded-2xl flex flex-col items-center justify-evenly"
  >
    <input
      v-model="userToDo"
      @keyup.enter="addToList(userToDo)"
      type="text"
      class="rounded-2xl focus:outline-none focus:ring-2 focus:ring-white bg-black text-white w-[90%] pl-4 h-[10%]"
      placeholder="Type your task."
    />

    <div
      class="container w-full h-[50%] flex flex-col items-center overflow-auto gap-3.5"
    >
      <div
        v-for="(todo, index) in list"
        :key="index"
        class="task text-white w-[90%] flex items-center gap-5 align-text-bottom"
        @mouseenter="hoveredTask = index"
        @mouseleave="hoveredTask = null"
        @click="completeTask(todo)"
      >
        <div @click.stop="removeTask(index)">
          <i
            v-if="hoveredTask === index"
            class="fa-solid fa-minus transition duration-200 cursor-pointer"
          ></i>
          <i
            v-else
            class="fa-solid fa-leaf transition duration-200 cursor-pointer"
          ></i>
        </div>

        <h1
          class="break-all cursor-pointer"
          :class="{ 'line-through': todo.done }"
        >
          {{ todo.description }}
        </h1>
      </div>
    </div>

    <div
      class="container w-[90%] rounded-2xl h-fit p-3 flex justify-evenly  bg-[#0000007d] items-center "
    >

      <svg
        class="w-20 h-20"
        viewBox="0 0 100 100"
      >
        <!-- Background circle -->
        <circle
          cx="50"
          cy="50"
          r="45"
          stroke="#e5e7eb"
          stroke-width="10"
          fill="none"
        />
        <!-- Progress circle -->
        <circle
          cx="50"
          cy="50"
          r="45"
          :stroke="progressColor"
          stroke-width="10"
          fill="none"
          stroke-linecap="round"
          :stroke-dasharray="circumference"
          :stroke-dashoffset="dashOffset"
          transform="rotate(-90 50 50)"
          class="transition-all duration-500"
        />
        <text
          x="50"
          y="58"
          text-anchor="middle"
          font-size="25"
          :fill="progressColor"
          font-weight="bold"
        >
          {{ percentCompleted }}%
        </text>
      </svg>
        <h2 class="font-semibold text-2xl text-[#e5e7eb] ">{{doneCount}}/{{ list.length}} tasks completed</h2>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue';
const userToDo = ref('');
const list = ref([]);
const hoveredTask = ref(null);

function addToList(desc) {
  const trimmed = desc.trim();
  if (!trimmed) return; // Do nothing if empty
  list.value.push({
    description: trimmed,
    done: false,
  });
  userToDo.value = '';
}
function removeTask(index) {
  list.value.splice(index, 1);
}
function completeTask(todo) {
  todo.done = !todo.done;
}

// Circle progress calculations
const radius = 45;
const circumference = 2 * Math.PI * radius;
const doneCount = ref(0);

const percentCompleted = computed(() => {
  if (list.value.length === 0) {
    doneCount.value = 0;
    return 0;
  }
  doneCount.value = list.value.filter((t) => t.done).length;
  return Math.round((doneCount.value / list.value.length) * 100);
});


const dashOffset = computed(() => {
  return circumference * (1 - percentCompleted.value / 100);
});

const progressColor = computed(() => {
  // amber color if < 100%, green if completed
  return percentCompleted.value === 100 ? '#22c55e' : '#f59e0b'; // Tailwind green-500 and amber-500
});
</script>

<style scoped>
/* You can add extra styling here if needed */
</style>
