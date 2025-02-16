<template>
  <div class="game-container">
    <h2>Juego de Dibujo Libre</h2>
    <div class="color-palette">
      <button v-for="color in colors" :key="color" :style="{ backgroundColor: color }" @click="selectColor(color)"></button>
    </div>
    <div class="brush-size">
      <label for="brushSize">Grosor del Pincel:</label>
      <select v-model="lineWidth" id="brushSize">
        <option v-for="size in brushSizes" :key="size" :value="size">{{ size }}</option>
      </select>
    </div>
    <div class="brush-shape">
      <label for="brushShape">Forma del Pincel:</label>
      <select v-model="brushShape" id="brushShape">
        <option value="round">Redondo</option>
        <option value="square">Cuadrado</option>
      </select>
    </div>
    <canvas ref="gameCanvas" width="600" height="600" @mousedown="startDrawing" @mouseup="stopDrawing" @mousemove="draw"></canvas>
    <p>¡Dibuja algo!</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDrawing: false, // Estado de dibujo
      lastX: 0, // Última posición X
      lastY: 0, // Última posición Y
      strokeColor: 'black', // Color del trazo
      colors: ['black', 'red', 'green', 'blue', 'yellow', 'purple', 'orange'], // Paleta de colores
      lineWidth: 2, // Grosor del pincel
      brushSizes: [1, 2, 4, 6, 8, 10], // Opciones de grosor del pincel
      brushShape: 'round', // Forma del pincel
    };
  },
  methods: {
    selectColor(color) {
      this.strokeColor = color; // Actualiza el color del trazo
    },
    startDrawing(event) {
      this.isDrawing = true; // Inicia el estado de dibujo
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const rect = canvas.getBoundingClientRect(); // Obtiene el rectángulo del canvas
      this.lastX = event.clientX - rect.left; // Guarda la posición inicial X
      this.lastY = event.clientY - rect.top; // Guarda la posición inicial Y
    },
    stopDrawing() {
      this.isDrawing = false; // Detiene el estado de dibujo
    },
    draw(event) {
      if (!this.isDrawing) return; // Si no está dibujando, no hace nada

      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
      const rect = canvas.getBoundingClientRect(); // Obtiene el rectángulo del canvas

      ctx.strokeStyle = this.strokeColor; // Color del trazo
      ctx.lineWidth = this.lineWidth; // Grosor del trazo
      ctx.beginPath(); // Inicia un nuevo camino
      ctx.moveTo(this.lastX, this.lastY); // Mueve a la última posición
      this.lastX = event.clientX - rect.left; // Actualiza la posición X
      this.lastY = event.clientY - rect.top; // Actualiza la posición Y

      // Dibuja según la forma del pincel
      if (this.brushShape === 'round') {
        ctx.lineTo(this.lastX, this.lastY); // Dibuja una línea hasta la nueva posición
      } else if (this.brushShape === 'square') {
        ctx.rect(this.lastX - this.lineWidth / 2, this.lastY - this.lineWidth / 2, this.lineWidth, this.lineWidth); // Dibuja un cuadrado
      }
      ctx.stroke(); // Aplica el trazo
    },
  },
};
</script>

<style scoped>
.game-container {
  text-align: center;
  margin-top: 20px;
}
canvas {
  border: 1px solid #000; /* Borde del canvas */
}
.color-palette {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
}
.color-palette button {
  width: 30px;
  height: 30px;
  border: none;
  cursor: pointer;
  margin: 0 5px;
}
.brush-size {
  margin: 10px 0;
}
.brush-shape {
  margin: 10px 0;
}
</style> 