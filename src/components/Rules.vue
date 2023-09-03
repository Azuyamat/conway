<script setup>

</script>

<template>
  <ul>
    <li>
      <span>💓</span>
      <p>A dead cell with exactly three live neighbors revives.</p>
    </li>
    <li>
      <span>⚓</span>
      <p>A live cell with two or three live neighbors persists.</p>
    </li>
    <li>
      <span>🏴‍☠️</span>
      <p>A live cell with more than three live neighbors dies.</p>
    </li>
    <li>
      <span>🏴‍☠️</span>
      <p>A live cell with fewer than two live neighbors dies.</p>
    </li>
  </ul>
</template>

<style scoped>
  ul{
    list-style-type: none;
    display: flex;
    flex-direction: column;
    gap:30px;
    align-items: center;
  }
  ul li{
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    gap: 30px;
    background: rgba(71, 255, 157, 0.9);
    border-radius: 10px;
    border: 2px solid #47FFB2FF;
    color: black;
    width: fit-content;
    padding: 15px 40px;
  }
</style>