<script setup>
const props = defineProps({
  value: {
    type: Number,
    default: 0
  },
  col: {
    type: Number,
    required: true
  }
})

const emit = defineEmits(['drop'])

function handleClick() {
  emit('drop', props.col)
}
</script>

<template>
  <div
    class="cell"
    :class="{
      'cell--player1': value === 1,
      'cell--player2': value === 2
    }"
    @click="handleClick"
  />
</template>

<style scoped>
.cell {
  width: var(--cell-size);
  height: var(--cell-size);
  border-radius: 50%;
  background: var(--cell-empty);
  cursor: pointer;
  transition: background 0.2s ease;
}

.cell--player1 {
  background: var(--cell-player1);
}

.cell--player2 {
  background: var(--cell-player2);
}
</style>
