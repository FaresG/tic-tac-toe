<script setup>
import Title from '@/components/Title.vue'
import {reactive} from "vue";
const playerX = reactive({
  turn: false,
  ownedPositions: [],
})
const playerO = reactive({
  turn: false,
  ownedPositions: [],
})
</script>
<template>
  <div class="w-screen h-screen">
    <div class="px-[20vw] flex flex-col gap-4 py-10 w-full h-full">
      <Title name="Play Tic Tac Toe"/>
      <div class="tic-tac-toe grid-cols-3 grid grid-rows-3">
        <div v-for="box in 9" :key="box" class="border aspect-square"></div>
      </div>
    </div>
  </div>
</template>
