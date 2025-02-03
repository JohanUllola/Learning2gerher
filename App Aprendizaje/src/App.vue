<script setup>
import HelloWorld from './components/GameBoard.vue';
</script>

<template>
  <div id="app">
   
    <GameBoard />
    
  </div>
</template>

<script>
import GameBoard from './components/GameBoard.vue';

export default {
  components: {
    GameBoard,
  },
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f0f8ff;
  display:flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

#app {
  text-align: center;
}

.instructions {
  font-size: 1.5rem;
  color: #333;
}
</style>
