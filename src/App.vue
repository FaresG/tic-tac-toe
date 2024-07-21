<script setup>
import {computed, reactive, ref} from "vue";
import { XMarkIcon, CheckIcon } from '@heroicons/vue/24/solid'
import Title from '@/components/Title.vue'

const winCombinations = [
  [1, 2, 3], // Top row
  [4, 5, 6], // Middle row
  [7, 8, 9], // Bottom row
  [1, 4, 7], // Left column
  [2, 5, 8], // Middle column
  [3, 6, 9], // Right column
  [1, 5, 9], // Diagonal from top-left to bottom-right
  [3, 5, 7]  // Diagonal from top-right to bottom-left
];

const playerX = reactive({
  id: 'playerX',
  turn: false,
  ownedBoxes: [],
})
const playerO = reactive({
  id: 'playerO',
  turn: true, // player O always start
  ownedBoxes: [],
})

const currentPlayer = computed(() => {
  if (playerX.turn) return playerX
  else return playerO
})

const isBoxPlayed = (box) => {
  return ([...playerX.ownedBoxes, ...playerO.ownedBoxes]).find((ownedBox) => ownedBox === box)
}

const winner = ref()
const playBox = (box) => {
  if (winner.value) return
  if (isBoxPlayed(box)) {
    // you can't play that
    return
  }
  if (isDraw.value) return

  currentPlayer.value.ownedBoxes.push(box)
  // check if game is done
  if (hasWon(currentPlayer.value)) {
    winner.value = currentPlayer.value?.id
    return
  }

  if (currentPlayer.value.id === playerX.id) {
    playerO.turn = true
    playerX.turn = false
  } else {
    playerO.turn = false
    playerX.turn = true
  }
}

const clickedByPlayerX = computed(() => {
  return (box) => playerX.ownedBoxes.find((ownedBox) => ownedBox === box)
})

const clickedByPlayerO = computed(() => {
  return (box) => playerO.ownedBoxes.find((ownedBox) => ownedBox === box)
})

const hasWon = (player) => {
  let result = false
  winCombinations.map((combination) => {
    if (combination.every(position => player.ownedBoxes.includes(position)))
      result = true
  })
  return result
}

const isDraw = computed(() => {
  return ((playerX.ownedBoxes.length + playerO.ownedBoxes.length) === 9 && !winner.value)
})

const restart = () => {
  playerX.turn = false
  playerX.ownedBoxes = []
  playerO.turn = true
  playerO.ownedBoxes = []
  currentPlayer.value = []
  winner.value = null
}
</script>
<template>
  <div class="w-screen h-screen">
    <div class="px-[20vw] flex flex-col gap-4 py-10 w-full h-full">
      <Title name="Play Tic Tac Toe" styling="mb-10"/>
      <Title
          v-show="winner"
          styling="text-green-500 text-xl"
          :name="`${winner} won the game!`"
      />
      <Title
          v-show="isDraw"
          styling="text-green-500 text-xl"
          name="It's a Draw!"
      />
      <div class="w-full flex justify-center">
        <button
            v-show="winner || isDraw"
            class="btn btn-accent"
            @click="restart"
        >Restart</button>
      </div>
      <div class="tic-tac-toe grid-cols-3 grid grid-rows-3">
        <div
            v-for="box in 9"
            :key="box"
            class="border aspect-square hover:cursor-pointer"
            @click="playBox(box)"
            :class="{'opacity-50': winner}"
        >
          <XMarkIcon class="text-red-500" v-show="clickedByPlayerX(box)"/>
          <CheckIcon class="text-amber-500" v-show="clickedByPlayerO(box)"/>
        </div>
      </div>
    </div>
  </div>
</template>
