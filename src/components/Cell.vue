<template>
  <div
      class="cell"
      :style="{ backgroundColor: getCellColor(nerd, age) }"
      @click="$emit('click')"
      @mousemove="$emit('mousemove')"
  />
</template>

<script>
export default {
  props: {
    alive: Number, // Pass the alive prop
    age: Number,   // Pass the age prop
    nerd: String
  },
  methods: {
    getCellColor(nerdCell, ageCell) {
      if (nerdCell===0){
        return 'rgb(255,255,255)'
      }
      const r = Math.min(255,100 + ageCell * 10);
      const g = 244;
      const b = Math.min(255,100 + ageCell * 10);
      return `rgb(${r}, ${g}, ${b})`;
    },
  },
};
</script>

<style scoped>
.cell {
  --len:2.5vw;
  width: var(--len);
  height: var(--len);
  border: 1px solid #ccc;
  cursor: pointer;
  transition: all 500ms linear;
}
</style>
