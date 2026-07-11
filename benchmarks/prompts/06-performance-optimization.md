You are a Staff Frontend Engineer reviewing a Vue 3 + TypeScript component for performance.

Analyze the following component and suggest performance optimizations.

```
<script setup lang="ts">
import { computed, defineComponent, ref, type PropType } from "vue";
type Cell = "red" | "yellow" | null;

const ROWS = 6;
const initialTurn: Cell = "red";
const COLS = 7;
const CONSECUTIVE_TO_WIN = 4;
// Direction vectors are [rowDelta, colDelta].
const DIRECTIONS: Array<[number, number]> = [
  [0, 1], // horizontal (left/right)
  [1, 0], // vertical (down/up)
  [1, 1], // primary diagonal (\)
  [1, -1], // secondary diagonal (/)
];

const turns = ref<Cell>(initialTurn);
const isPlaying = ref<Boolean>(false);
const winner = ref<Cell>(null);

const createEmptyBoard = () =>
  Array.from({ length: ROWS }, () => Array<Cell>(COLS).fill(null));

const boardstate = ref<Array<Array<Cell>>>(createEmptyBoard());

const sharedGridStyle = {
  display: "grid",
  gridTemplateColumns: `repeat(${COLS}, [col-start] 1fr)`,
};

const onReset = () => {
  isPlaying.value = false;
  boardstate.value = createEmptyBoard();
  turns.value = initialTurn;
  winner.value = null;
};

const updateBoardState = (colId: number): number => {
  const colIndex = colId - 1;
  let rowIndex = -1;
  for (let i = ROWS - 1; i >= 0; i--) {
    const row = boardstate.value[i];
    if (!row) {
      continue;
    }

    if (row[colIndex] === null) {
      row[colIndex] = turns.value;
      rowIndex = i;
      break;
    }
  }

  return rowIndex;
};

const isInBounds = (row: number, col: number): boolean => {
  return row >= 0 && row < ROWS && col >= 0 && col < COLS;
};

const countDirection = (
  row: number,
  col: number,
  dx: number,
  dy: number,
  value: Cell,
): number => {
  let count = 0;
  let r = row + dx;
  let c = col + dy;

  while (isInBounds(r, c)) {
    const currentRow = boardstate.value[r];
    if (!currentRow || currentRow[c] !== value) {
      break;
    }

    count++;
    r += dx;
    c += dy;
  }

  return count;
};

const checkWin = (colId: number, rowIndex: number): boolean => {
  const colIndex = colId - 1;
  const value = turns.value;

  for (const [dx, dy] of DIRECTIONS) {
    const forward = countDirection(rowIndex, colIndex, dx, dy, value);
    const backward = countDirection(rowIndex, colIndex, -dx, -dy, value);

    const total = 1 + forward + backward;

    if (total >= CONSECUTIVE_TO_WIN) {
      return true;
    }
  }

  return false;
};

const onSelect = (colId: number) => {
  if (!isPlaying.value) {
    isPlaying.value = true;
  }

  const rowIndex = updateBoardState(colId);
  if (checkWin(colId, rowIndex)) {
    winner.value = turns.value;
    return;
  }
  turns.value = turns.value === "red" ? "yellow" : "red";
};

const Selection = defineComponent({
  props: {
    onSelect: {
      type: Function as PropType<(colId: number) => void>,
      required: true,
    },
  },
  setup(props) {
    const cols = COLS;

    const selectionClass = computed(() => {
      return turns.value === "red"
        ? "selection-circle-red"
        : "selection-circle-yellow";
    });
    return {
      cols,
      sharedGridStyle,
      onSelect: props.onSelect,
      selectionClass,
    };
  },
  template: `<div class="selection" :style="sharedGridStyle">
    <div class="selection-item" v-for="n in cols" :key="n" @click="() => onSelect(n)">
      <div class="circle selection-circle" :class="selectionClass"></div>
    </div>
  </div>`,
});

const Board = defineComponent({
  components: { Selection },
  props: {
    boardstate: {
      type: Array as () => Array<Array<"red" | "yellow" | null>>,
      required: true,
    },
    onSelect: {
      type: Function as PropType<(colId: number) => void>,
      required: true,
    },
  },
  setup(props) {
    const cellClass = computed(() =>
      props.boardstate.flat().map((cell) => (cell === null ? "white" : cell)),
    );
    return {
      rows: ROWS,
      cols: COLS,
      sharedGridStyle,
      cellClass,
    };
  },
  template: `
  <div class="board-stack">
    <Selection :onSelect="onSelect" />
    <div class="board blue" :style="sharedGridStyle">
      <div v-for="(cls, i) in cellClass" :key="i" class="grid-item">
        <div class="circle" :class="cls"></div>
      </div>
    </div>
  </div>
`,
});

const Controls = defineComponent({
  setup() {
    return {
      onReset,
    };
  },
  template: `<button class="controls" @click="onReset">
    Reset
  </button>`,
});

const Results = defineComponent({
  props: {
    winner: {
      type: String as () => Cell,
      required: true,
    },
  },
  setup(props) {
    const winnerClass = computed(() => {
      switch (props.winner) {
        case "red":
          return "red";
        case "yellow":
          return "yellow";
        default:
          return "";
      }
    });
    return {
      winnerClass,
    };
  },
  template: `<div class="results" v-if="winner">
    <div class="winner circle" :class="winnerClass">Won</div>
  </div>`,
});
</script>

<template>
  <div class="connect-four-container">
    <Board :boardstate="boardstate" :onSelect="onSelect" />
    <Controls />
    <Results :winner="winner" />
  </div>
</template>

<style scoped>
.connect-four-container {
  --board-width: 500px;
  --board-gap: 10px;
  --yellow: #f1c40f;
  --red: #e74c3c;
  --white: #ecf0f1;

  width: 100%;
  height: 90vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

:deep(.board-stack) {
  width: var(--board-width);
}

:deep(.board) {
  width: 100%;
  min-height: 450px;
  border-radius: 10px;
  gap: var(--board-gap);
  padding: var(--board-gap);
  box-sizing: border-box;
  margin-bottom: 10px;
}

:deep(.controls) {
  padding: 10px;
  border-radius: 15px;
  border: 0px;
  background-color: lightgrey;
  color: black;
}

:deep(.circle) {
  border-radius: 50%;
  width: 100%;
  aspect-ratio: 1 / 1;
  height: 50px;
  width: 50px;
}

:deep(.grid-item) {
  display: flex;
  align-items: center;
  justify-content: center;
}

:deep(.white) {
  background-color: var(--white);
}

:deep(.blue) {
  background-color: blue;
}

:deep(.red) {
  background-color: var(--red);
}

:deep(.red:hover) {
  background-color: var(--red);
}

:deep(.yellow) {
  background-color: var(--yellow);
}

:deep(.yellow:hover) {
  background-color: var(--yellow);
}

:deep(.controls:hover) {
  background-color: grey;
  cursor: pointer;
  color: white;
}

:deep(.selection) {
  width: 100%;
  gap: var(--board-gap);
  padding: 0 var(--board-gap);
  margin-bottom: 6px;
  box-sizing: border-box;
}

:deep(.selection-item) {
  display: flex;
  align-items: center;
  justify-content: center;
}

:deep(.selection-circle:hover) {
  cursor: pointer;
}

:deep(.selection-circle-red:hover) {
  background-color: red;
}

:deep(.selection-circle-yellow:hover) {
  background-color: yellow;
}

:deep(.results) {
  margin-top: 10px;
}

:deep(.winner) {
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: bold;
}
</style>

```

Requirements:

- Do NOT change the behavior.
- Do NOT introduce new features.
- Do NOT rewrite the whole component.
- Focus only on realistic performance improvements.
- Avoid premature optimization.
- Explain whether each optimization is actually worth doing.
- Mention if the current code is already performant enough.
- Keep Vue 3 Composition API best practices in mind.
- Keep TypeScript best practices in mind.

Return your answer in the following format:

## Performance Analysis

Explain the main performance characteristics of the component.

## Recommended Optimizations

For each recommendation include:

- Problem
- Why it matters
- Severity: Low / Medium / High
- Suggested fix
  If you believe a suggested optimization would not provide any measurable benefit for this component, explicitly say so.

## Optimized Code

```vue
// only include the changed parts, not the full component
```

## Optimizations intentionally NOT recommended

List optimizations you avoided because they would be premature, unnecessary, or would make the code harder to maintain.
