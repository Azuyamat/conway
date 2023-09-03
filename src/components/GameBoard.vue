<template>
  <div class="container">
    <div class="conway-container" draggable="false">
      <div
          v-for="(row, rowIndex) in grid"
          :key="rowIndex"
          class="row"
      >
        <Cell
            v-for="(cell, colIndex) in row"
            :key="colIndex"
            class="cell"
            :class="{ alive: cell === 1 }"
            :age="cellAges[rowIndex][colIndex]"
            :nerd="cell"
        @click="cellClick(rowIndex, colIndex)"
        @mousemove="cellMouseMove(rowIndex, colIndex)"
        ></Cell>
      </div>
    </div>
    <div class="buttons">
      <button @click="toggleDrawingMode">{{ drawingMode ? 'Drawing Mode: On' : 'Drawing Mode: Off' }}</button>
      <button @click="startStopGame">{{ running ? 'Stop' : 'Start' }}</button>
      <button @click="clearRandomize">{{ running ? 'Clear' : 'Randomize' }}</button>
    </div>
  </div>
</template>

<script>
import Cell from "./Cell.vue";

export default {
  components: { Cell },
  data() {
    return {
      rows: 30,
      cols: 30,
      grid: [],
      cellAges: [], // Add cellAges to track cell ages
      running: false,
      intervalId: null,
      drawingMode: false,
    };
  },
  mounted() {
    this.initializeGrid();
  },
  methods: {
    initializeGrid() {
      this.grid = Array.from({ length: this.rows }, () =>
          Array.from({ length: this.cols }, () => 0)
      );

      // Initialize cellAges array with zeros
      this.cellAges = Array.from({ length: this.rows }, () =>
          Array.from({ length: this.cols }, () => 0)
      );
    },
    toggleDrawingMode() {
      this.drawingMode = !this.drawingMode;
    },
    cellClick(row, col) {
      if (this.drawingMode) {
        this.grid[row][col] = this.grid[row][col] === 0 ? 1 : 0;
      }
    },
    cellMouseMove(row, col) {
      if (this.drawingMode && event.buttons === 1) {
        this.grid[row][col] = 1;
      }
    },
    startStopGame() {
      this.running = !this.running;
      if (this.running) {
        this.intervalId = setInterval(this.updateGrid, 200);
      } else {
        clearInterval(this.intervalId);
        this.intervalId = null;
      }
    },
    clearRandomize() {
      if (this.running) {
        for (let row = 0; row < this.rows; row++) {
          for (let col = 0; col < this.cols; col++) {
            this.grid[row][col] = 0;
            this.cellAges[row][col] = 0; // Reset ages
          }
        }
      } else {
        for (let row = 0; row < this.rows; row++) {
          for (let col = 0; col < this.cols; col++) {
            // Generate a random value (0 or 1) for each cell
            this.grid[row][col] = Math.round(Math.random());
            this.cellAges[row][col] = 0; // Initialize ages to 0
          }
        }
      }
      this.startStopGame();
    },
    updateGrid() {
      const newGrid = JSON.parse(JSON.stringify(this.grid));
      const countLiveNeighbors = (row, col) => {
        let count = 0;
        for (let i = -1; i <= 1; i++) {
          for (let j = -1; j <= 1; j++) {
            if (i === 0 && j === 0) continue;
            const newRow = row + i;
            const newCol = col + j;
            if (
                newRow >= 0 &&
                newRow < this.rows &&
                newCol >= 0 &&
                newCol < this.cols
            ) {
              count += this.grid[newRow][newCol];
            }
          }
        }
        return count;
      };
      for (let row = 0; row < this.rows; row++) {
        for (let col = 0; col < this.cols; col++) {
          const cell = this.grid[row][col];
          const liveNeighbors = countLiveNeighbors(row, col);

          if (cell === 1) {
            if (liveNeighbors < 2 || liveNeighbors > 3) {
              newGrid[row][col] = 0; // Cell dies
            }
          } else {
            if (liveNeighbors === 3) {
              newGrid[row][col] = 1;
            }
          }

          // Update cell age
          if (cell === 1) {
            this.cellAges[row][col]++;
          } else {
            this.cellAges[row][col] = 0;
          }
        }
      }
      this.grid = newGrid;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.conway-container {
  display: inline-block;
  text-align: center;
  border-radius: 10px;
  overflow: hidden;
}
.row {
  display: flex;
}

.buttons{
  margin: 30px;
  display: flex;
  gap: 10px;
}
</style>
