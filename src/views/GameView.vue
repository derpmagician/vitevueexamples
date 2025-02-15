<template>
  <div class="game">
    <h2>Juego de Caza de Puntos</h2>
    <!-- Canvas donde se dibuja el juego -->
    <canvas ref="gameCanvas" width="400" height="400" @mousemove="updateMousePosition" @click="catchPoint"></canvas>
    <p>Puntos: {{ score }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      score: 0, // Puntuación del jugador
      point: { x: 0, y: 0 }, // Posición del punto rojo
      velocity: { x: 1, y: 0 }, // Velocidad inicial del punto
      animationFrameId: null, // ID del frame de animación
      mousePosition: { x: 0, y: 0 }, // Posición del mouse
      arcSize: 10, // Tamaño del arco azul
    };0
  },
  mounted() {
    this.drawPoint(); // Dibuja el punto rojo al inicio
    this.startAnimation(); // Inicia la animación del juego
  },
  beforeDestroy() {
    cancelAnimationFrame(this.animationFrameId); // Cancela la animación al destruir el componente
  },
  methods: {
    drawPoint() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
      ctx.clearRect(0, 0, canvas.width, canvas.height); // Limpia el canvas

      // Dibuja el punto rojo
      ctx.fillStyle = 'red';
      ctx.beginPath();
      ctx.arc(this.point.x, this.point.y, 15, 0, Math.PI * 2); // Dibuja un círculo rojo
      ctx.fill();

      // Dibuja el arco azul que sigue al mouse
      ctx.strokeStyle = 'blue'; // Color del arco
      ctx.lineWidth = 2; // Grosor del arco
      ctx.beginPath();
      ctx.arc(this.mousePosition.x, this.mousePosition.y, this.arcSize, 0, Math.PI * 2); // Dibuja un círculo azul con tamaño variable
      ctx.stroke();
    },
    catchPoint(event) {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const rect = canvas.getBoundingClientRect(); // Obtiene el rectángulo del canvas
      const x = event.clientX - rect.left; // Coordenada X del clic
      const y = event.clientY - rect.top; // Coordenada Y del clic

      // Verifica si el arco azul toca el punto rojo
      const dx = this.mousePosition.x - this.point.x; // Diferencia en X
      const dy = this.mousePosition.y - this.point.y; // Diferencia en Y
      if (Math.sqrt(dx * dx + dy * dy) < 15 + this.arcSize) { // Verifica si el arco azul toca el punto rojo
        this.score++; // Incrementa la puntuación
        this.updateVelocity(); // Aumenta la velocidad si es necesario
        this.changePointPosition(); // Cambia la posición y dirección del punto
        this.drawPoint(); // Dibuja el nuevo punto
      }
    },
    startAnimation() {
      this.changePointPosition(); // Genera posición aleatoria al inicio

      const animate = () => {
        this.updatePosition(); // Actualiza la posición del punto
        this.drawPoint(); // Dibuja el punto y el arco
        this.animationFrameId = requestAnimationFrame(animate); // Solicita el siguiente frame de animación
      };
      animate(); // Inicia la animación
    },
    updatePosition() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      this.point.x += this.velocity.x; // Actualiza la posición en X
      this.point.y += this.velocity.y; // Actualiza la posición en Y

      // Verifica colisiones con los bordes
      if (this.point.x <= 10 || this.point.x >= canvas.width - 10) {
        this.velocity.x = -this.velocity.x; // Rebotar en el eje X
      }
      if (this.point.y <= 10 || this.point.y >= canvas.height - 10) {
        this.velocity.y = -this.velocity.y; // Rebotar en el eje Y
      }
    },
    updateVelocity() {
      if (this.score % 10 === 0) { // Cada 10 puntos
        this.velocity.x *= 1.1; // Aumenta la velocidad en el eje X
        this.velocity.y *= 1.1; // Aumenta la velocidad en el eje Y
      }
    },
    changePointPosition() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      // Genera nueva posición aleatoria
      this.point.x = Math.random() * (canvas.width - 20) + 10; // Nueva posición X
      this.point.y = Math.random() * (canvas.height - 20) + 10; // Nueva posición Y

      // Genera nueva dirección aleatoria
      this.velocity.x = (Math.random() < 0.5 ? 1 : -1) * (Math.random() * 2 + 1); // Nueva dirección X
      this.velocity.y = (Math.random() < 0.5 ? 1 : -1) * (Math.random() * 2 + 1); // Nueva dirección Y
    },
    updateMousePosition(event) {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const rect = canvas.getBoundingClientRect(); // Obtiene el rectángulo del canvas
      this.mousePosition.x = event.clientX - rect.left; // Actualiza la posición X del mouse
      this.mousePosition.y = event.clientY - rect.top; // Actualiza la posición Y del mouse
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
  border: 1px solid #000; /* Borde del canvas */
}
</style> 