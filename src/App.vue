<template>
  <main class="min-h-screen pt-8 text-center dark:bg-gray-800 dark:text-white">
    <h1 class="mb-8 text-6xl font-bold uppercase">Tic Tac Toe</h1>
    <h3 class="mb-4 text-xl">Player {{ player }}'s turn</h3>
    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(col, y) in row"
          :key="y"
          @click="makeMove(x, y)"
          :class="`border border-white w-20 h-20
          hover:bg-gray-700 flex items-center
          justify-center material-icons-outlined
          text-4xl cursor-pointer ${
            col === 'X' ? 'text-blue-500' : 'text-red-500'
          }`"
        >
          {{ col === "X" ? "close" : col === "O" ? "circle" : "" }}
        </div>
      </div>
      <h2 v-if="winner && winner !== 'Draw'" class="mb-8 text-6xl font-bold">
        Player '{{ winner }}' won
      </h2>
      <h2 v-else-if="winner === 'Draw'" class="mb-8 text-6xl font-bold">
        Draw, no one has won
      </h2>
      <button
        @click="ResetGame"
        class="px-4 py-2 mt-4 font-bold uppercase duration-300 bg-green-500 rounded hover:bg-green-600"
      >
        Reset Game
      </button>
    </div>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";
const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};
const winner = computed(() => {
  const result = calculateWinner(board.value.flat());
  if (
    board.value.flat().every((square) => square !== "") &&
    calculateWinner(board.value.flat()) === null
  )
    return "Draw";
  return result;
});
const makeMove = (x, y) => {
  if (winner.value) return;
  if (board.value[x][y] !== "") return;
  board.value[x][y] = player.value;
  player.value = player.value === "X" ? "O" : "X";
};
const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>
