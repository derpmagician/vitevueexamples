<template>
  <div class="game">
    <h2>Juego de Caza de Puntos</h2>
    <canvas ref="gameCanvas" width="400" height="400" @click="catchPoint"></canvas>
    <p>Puntos: {{ score }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      score: 0,
      point: { x: 0, y: 0 },
    };
  },
  mounted() {
    this.drawPoint();
  },
  methods: {
    drawPoint() {
      const canvas = this.$refs.gameCanvas;
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0, 0, canvas.width, canvas.height); // Limpiar el canvas
      this.point.x = Math.random() * (canvas.width - 20) + 10; // Generar posición aleatoria
      this.point.y = Math.random() * (canvas.height - 20) + 10;
      ctx.fillStyle = 'red';
      ctx.beginPath();
      ctx.arc(this.point.x, this.point.y, 10, 0, Math.PI * 2);
      ctx.fill();
    },
    catchPoint(event) {
      const canvas = this.$refs.gameCanvas;
      const rect = canvas.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      // Verificar si el clic está dentro del punto
      const dx = x - this.point.x;
      const dy = y - this.point.y;
      if (Math.sqrt(dx * dx + dy * dy) < 10) {
        this.score++;
        this.drawPoint(); // Dibujar un nuevo punto
      }
    },
  },
};
</script>

<style scoped>
.game {
  text-align: center;
  margin-top: 20px;
}
canvas {
  border: 1px solid #000;
}
</style> 