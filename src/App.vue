<template>
  <div>
    <h1>Conway's Game of Life</h1>
    <GameBoard/>
    <Rules/>
  </div>
</template>

<script setup>
  import GameBoard from "./components/GameBoard.vue";
  import Rules from "./components/Rules.vue";
</script>
