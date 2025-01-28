<template>
  <div id="app">
    <h1>Asocia los Colores</h1>
    <p class="instructions">Arrastra el cuadro al color correspondiente</p>
    <div class="game-board" v-if="!gameOver">
      <div 
        class="draggable" 
        v-for="(color, index) in draggableColors" 
        :key="`draggable-${index}`"
        draggable="true" 
        @dragstart="startDrag($event, color)"
        :style="{ backgroundColor: color }"
      >
      </div>
      <div 
        class="drop-zone" 
        v-for="(color, index) in dropGrid" 
        :key="`dropzone-${index}`"
        @dragover.prevent 
        @drop="handleDrop($event, index)"
        :style="{ backgroundColor: color }"
      >
      </div>
    </div>
    <p class="score" v-if="!gameOver">Puntuación: {{ score }}</p>

    <div v-if="gameOver">
      <h2>¡Has alcanzado 100 puntos!</h2>
      <button @click="restartGame">Reiniciar Juego</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      score: 0,
      gameOver: false,
      draggedColor: null,
      draggableColors: this.generateInitialColors(),
      dropGrid: this.generateInitialColors()
    };
  },
  methods: {
    generateInitialColors() {
      const colors = ["#ff6f61", "#00bcd4", "#4caf50", "#ffeb3b", "#9c27b0", "#ff9800", "#03a9f4", "#8bc34a", "#e91e63"];
      return [
        colors[Math.floor(Math.random() * colors.length)],
        colors[Math.floor(Math.random() * colors.length)],        
        colors[Math.floor(Math.random() * colors.length)]
      ];
    },
    startDrag(event, color) {
      this.draggedColor = color;
      event.dataTransfer.setData('color', color);
    },
    handleDrop(event, dropIndex) {
      const dropColor = this.dropGrid[dropIndex];

      if (this.draggedColor === dropColor) {
        this.score += 10;

        // Verificar si se ha alcanzado la puntuación de 100 puntos
        if (this.score >= 100) {
          this.gameOver = true;
          return;
        }

        // Reemplazar colores con nuevos aleatorios tras acierto
        const newDraggableColor = this.getRandomColor();
        const newDropColor = this.getRandomColor();
        const draggableIndex = this.draggableColors.indexOf(this.draggedColor);

        if (draggableIndex !== -1) {
          this.draggableColors.splice(draggableIndex, 1, newDraggableColor);
        }
        this.dropGrid.splice(dropIndex, 1, newDropColor);
      } else {
        alert('Color incorrecto, intenta de nuevo.');
      }

      this.draggedColor = null; // Reset para evitar conflictos
    },
    getRandomColor() {
      const colors = ["#ff6f61", "#00bcd4", "#4caf50", "#ffeb3b", "#9c27b0", "#ff9800", "#03a9f4", "#8bc34a", "#e91e63"];
      return colors[Math.floor(Math.random() * colors.length)];
    },
    restartGame() {
      this.score = 0;
      this.gameOver = false;
      this.draggableColors = this.generateInitialColors();
      this.dropGrid = this.generateInitialColors();
    }
  }
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f0f8ff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

#app {
  text-align: center;
}

.game-board {
  display: grid;
  grid-template-columns: repeat(3, 100px); /* Cambiar la cuadrícula a 3 columnas */
  gap: 10px;
  justify-content: center;
  margin-top: 20px;
}

.drop-zone, .draggable {
  width: 100px;
  height: 100px;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.2rem;
  font-weight: bold;
  color: #fff;
  user-select: none;
}

.draggable {
  cursor: grab;
}

.instructions {
  font-size: 1.5rem;
  color: #333;
}

.score {
  font-size: 1.2rem;
  color: #555;
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #00bcd4;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0097a7;
}
</style>