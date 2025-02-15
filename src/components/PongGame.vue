<template>
  <div class="game-container">
    <h2>Juego de Pong</h2>
    <canvas ref="gameCanvas" width="400" height="400"></canvas>
    <p>Puntos: {{ score }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      score: 0, // Puntuación del jugador
      playerPaddle: { x: 10, y: 150, width: 10, height: 60, color: 'purple' }, // Propiedades de la paleta del jugador
      cpuPaddle: { x: 380, y: 150, width: 10, height: 60, color: 'blue' }, // Propiedades de la paleta de la CPU
      ball: { x: 200, y: 200, radius: 10, velocityX: 2, velocityY: 1 }, // Propiedades de la pelota
      animationFrameId: null, // ID del frame de animación
    };
  },
  mounted() {
    this.startAnimation(); // Inicia la animación del juego
    window.addEventListener('mousemove', this.movePlayerPaddle); // Escucha el movimiento del mouse
  },
  beforeDestroy() {
    cancelAnimationFrame(this.animationFrameId); // Cancela la animación al destruir el componente
    window.removeEventListener('mousemove', this.movePlayerPaddle); // Elimina el listener
  },
  methods: {
    drawPaddle(paddle) {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
      ctx.fillStyle = paddle.color; // Usa el color de la paleta
      ctx.fillRect(paddle.x, paddle.y, paddle.width, paddle.height); // Dibuja la paleta
    },
    drawBall() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
      ctx.fillStyle = 'red';
      ctx.beginPath();
      ctx.arc(this.ball.x, this.ball.y, this.ball.radius, 0, Math.PI * 2); // Dibuja la pelota
      ctx.fill();
    },
    updateBallPosition() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      this.ball.x += this.ball.velocityX; // Actualiza la posición en X
      this.ball.y += this.ball.velocityY; // Actualiza la posición en Y

      // Verifica colisiones con los bordes
      if (this.ball.y + this.ball.radius > canvas.height || this.ball.y - this.ball.radius < 0) {
        this.ball.velocityY = -this.ball.velocityY; // Rebota en el eje Y
      }

      // Verifica colisiones con la paleta del jugador
      if (this.ball.x - this.ball.radius < this.playerPaddle.x + this.playerPaddle.width &&
          this.ball.y > this.playerPaddle.y && this.ball.y < this.playerPaddle.y + this.playerPaddle.height) {
        this.ball.velocityX = -this.ball.velocityX; // Rebota en el eje X
        this.score++; // Incrementa la puntuación
      }

      // Verifica colisiones con la paleta de la CPU
      if (this.ball.x + this.ball.radius > this.cpuPaddle.x &&
          this.ball.y > this.cpuPaddle.y && this.ball.y < this.cpuPaddle.y + this.cpuPaddle.height) {
        this.ball.velocityX = -this.ball.velocityX; // Rebota en el eje X
      }

      // Verifica si la pelota sale del canvas
      if (this.ball.x + this.ball.radius > canvas.width) {
        this.resetGame(); // Reinicia el juego si la pelota sale
      }
    },
    movePlayerPaddle(event) {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const rect = canvas.getBoundingClientRect(); // Obtiene el rectángulo del canvas
      const mouseY = event.clientY - rect.top; // Posición Y del mouse

      // Mueve la paleta del jugador con el mouse
      this.playerPaddle.y = mouseY - this.playerPaddle.height / 2;
      if (this.playerPaddle.y < 0) this.playerPaddle.y = 0; // Limita la paleta al borde superior
      if (this.playerPaddle.y + this.playerPaddle.height > canvas.height) this.playerPaddle.y = canvas.height - this.playerPaddle.height; // Limita la paleta al borde inferior
    },
    moveCPUPaddle() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      // Mueve la paleta de la CPU para que su centro esté en la pelota
      this.cpuPaddle.y = this.ball.y - this.cpuPaddle.height / 2;

      // Limita la paleta de la CPU al borde del canvas
      if (this.cpuPaddle.y < 0) this.cpuPaddle.y = 0;
      if (this.cpuPaddle.y + this.cpuPaddle.height > canvas.height) this.cpuPaddle.y = canvas.height - this.cpuPaddle.height;
    },
    resetGame() {
      this.score = 0; // Reinicia la puntuación
      this.ball.x = 200; // Reinicia la posición de la pelota
      this.ball.y = 200;
      this.ball.velocityX = 2; // Reinicia la velocidad de la pelota
      this.ball.velocityY = 1;
    },
    startAnimation() {
      const animate = () => {
        const canvas = this.$refs.gameCanvas; // Obtiene el canvas
        const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height); // Limpia el canvas
        this.drawPaddle(this.playerPaddle); // Dibuja la paleta del jugador
        this.drawPaddle(this.cpuPaddle); // Dibuja la paleta de la CPU
        this.drawBall(); // Dibuja la pelota
        this.updateBallPosition(); // Actualiza la posición de la pelota
        this.moveCPUPaddle(); // Mueve la paleta de la CPU
        this.animationFrameId = requestAnimationFrame(animate); // Solicita el siguiente frame de animación
      };
      animate(); // Inicia la animación
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
</style> 