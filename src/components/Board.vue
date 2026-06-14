<script setup>
import Cell from './Cell.vue'

const props = defineProps({
  board: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['drop'])

function handleDrop(col) {
  emit('drop', col)
}
</script>

<template>
  <div class="board">
    <div
      v-for="(row, rowIndex) in board"
      :key="rowIndex"
      class="board__row"
    >
      <Cell
        v-for="(cell, colIndex) in row"
        :key="colIndex"
        :value="cell"
        :col="colIndex"
        @drop="handleDrop"
      />
    </div>
  </div>
</template>

<style scoped>
.board {
  background: var(--board-bg);
  border-radius: var(--board-border-radius);
  padding: var(--cell-gap);
  display: flex;
  flex-direction: column;
  gap: var(--cell-gap);
}

.board__row {
  display: flex;
  gap: var(--cell-gap);
}
</style>
