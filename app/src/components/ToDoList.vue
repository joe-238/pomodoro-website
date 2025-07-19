<template>
    <div class="h-[74%] bg-[#3c3c3cde] rounded-2xl flex flex-col items-center justify-evenly">
        <input v-model="userToDo" @keyup.enter="addToList(userToDo)" type="text" class="rounded-2xl bg-red-500 w-[90%] pl-4 h-[10%] focus: " placeholder="Type your task.">
        <div class="container w-full h-[80%] flex flex-col items-center overflow-auto gap-3.5">
            <div v-for="todo in list" class="task text-white w-[90%] flex items-center gap-5 align-text-bottom">
                <i class="fa-solid fa-leaf"></i>
                <h1 @click="" class="break-all ">{{todo.description}}</h1>
            </div>
        </div>
        
    </div>
</template>

<script setup>
import { ref } from 'vue';
const userToDo = ref('');
const list = ref([]);

function addToList(desc) {
  const trimmed = desc.trim();
  if (!trimmed) return; // Do nothing if empty
  list.value.push({
    description: trimmed,
    done: false
  });
  userToDo.value = '';
}

</script>

<style lang="scss" scoped>

</style>