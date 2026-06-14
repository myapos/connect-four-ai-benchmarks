<script setup>
import { ref, computed } from 'vue'
import Board from './components/Board.vue'

const ROWS = 6
const COLS = 7

function createEmptyBoard() {
  return Array.from({ length: ROWS }, () => Array(COLS).fill(0))
}

const board = ref(createEmptyBoard())
const currentPlayer = ref(1)
const winner = ref(null)
const isDraw = ref(false)

const statusMessage = computed(() => {
  if (winner.value) {
    return `Player ${winner.value} wins! 🎉`
  }
  if (isDraw.value) {
    return "It's a draw! 🤝"
  }
  return `Player ${currentPlayer.value}'s turn`
})

function dropPiece(col) {
  if (winner.value || isDraw.value) return

  // Find the lowest empty row in the column
  const newBoard = board.value.map(row => [...row])
  let row = -1
  for (let r = ROWS - 1; r >= 0; r--) {
    if (newBoard[r][col] === 0) {
      row = r
      break
    }
  }
  if (row === -1) return // Column is full

  newBoard[row][col] = currentPlayer.value
  board.value = newBoard

  if (checkWinner(newBoard, row, col, currentPlayer.value)) {
    winner.value = currentPlayer.value
    return
  }

  if (newBoard.every(r => r.every(cell => cell !== 0))) {
    isDraw.value = true
    return
  }

  currentPlayer.value = currentPlayer.value === 1 ? 2 : 1
}

function checkWinner(b, row, col, player) {
  return (
    checkDirection(b, row, col, player, 0, 1) ||  // horizontal
    checkDirection(b, row, col, player, 1, 0) ||  // vertical
    checkDirection(b, row, col, player, 1, 1) ||  // diagonal /
    checkDirection(b, row, col, player, 1, -1)    // diagonal \
  )
}

function checkDirection(b, row, col, player, dr, dc) {
  let count = 1
  count += countInDirection(b, row, col, player, dr, dc)
  count += countInDirection(b, row, col, player, -dr, -dc)
  return count >= 4
}

function countInDirection(b, row, col, player, dr, dc) {
  let count = 0
  let r = row + dr
  let c = col + dc
  while (r >= 0 && r < ROWS && c >= 0 && c < COLS && b[r][c] === player) {
    count++
    r += dr
    c += dc
  }
  return count
}

function resetGame() {
  board.value = createEmptyBoard()
  currentPlayer.value = 1
  winner.value = null
  isDraw.value = false
}
</script>

<template>
  <div class="container">
    <h1 class="container__title">Connect Four</h1>

    <div class="container__status" :class="`container__status--player${currentPlayer}`">
      {{ statusMessage }}
    </div>

    <Board :board="board" @drop="dropPiece" />

    <button class="container__reset" @click="resetGame">
      New Game
    </button>
  </div>
</template>

<style scoped>
/*
  CSS variables from :root (defined in style.css) are inherited here.
  Do NOT redefine CSS variables inside <style scoped> using :root or :host,
  as scoped styles apply an attribute selector that prevents :root from matching.
  Instead, define global CSS variables in the global style.css file.
*/
.container {
  max-width: var(--container-max-width);
  margin: 0 auto;
  padding: var(--container-padding);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.container__title {
  font-size: 2rem;
  font-weight: 700;
  color: var(--text-primary);
  margin: 0;
  letter-spacing: 1px;
}

.container__status {
  font-size: 1.1rem;
  padding: 10px 24px;
  border-radius: var(--status-border-radius);
  background: var(--status-bg);
  color: var(--text-secondary);
  font-weight: 500;
  min-width: 200px;
  text-align: center;
}

.container__status--player1 {
  color: var(--cell-player1);
}

.container__status--player2 {
  color: var(--cell-player2);
}

.container__reset {
  margin-top: 8px;
  padding: 10px 32px;
  font-size: 1rem;
  font-family: var(--font-family);
  background: var(--board-bg);
  color: var(--text-primary);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 600;
  transition: opacity 0.2s;
}

.container__reset:hover {
  opacity: 0.85;
}
</style>
