<template>
  <div class="game-container">
    <h2>
      <button @click="showGame" class="title-button">Juego de Snake</button>
      <button @click="toggleTutorial" class="tutorial-button">Descripción del Juego</button>
    </h2>
    <div v-if="isGameVisible">
      <canvas ref="gameCanvas" width="600" height="600"></canvas>
      <p>Puntos: {{ score }}</p>
    </div>
    <SnakeTutorial v-if="showTutorial" />
  </div>
</template>

<script>
import SnakeTutorial from './SnakeTutorial.vue'; // Importa el componente de tutorial

export default {
  components: {
    SnakeTutorial, // Registra el componente de tutorial
  },
  data() {
    return {
      score: 0, // Puntuación del jugador
      snake: [{ x: 100, y: 100 }], // Cuerpo de la serpiente
      direction: { x: 0, y: 0 }, // Dirección de la serpiente
      food: { x: 0, y: 0 }, // Posición de la comida
      animationFrameId: null, // ID del frame de animación
      gridSize: 20, // Tamaño de la cuadrícula
      speed: 100, // Velocidad de actualización en milisegundos
      showTutorial: false, // Controla la visibilidad del tutorial
      isGameVisible: true, // Controla la visibilidad del juego
    };
  },
  mounted() {
    this.changeFoodPosition(); // Cambia la posición de la comida al inicio
    this.startAnimation(); // Inicia la animación del juego
    window.addEventListener('keydown', this.changeDirection); // Escucha las teclas
  },
  beforeDestroy() {
    cancelAnimationFrame(this.animationFrameId); // Cancela la animación al destruir el componente
    window.removeEventListener('keydown', this.changeDirection); // Elimina el listener
  },
  methods: {
    drawSnake() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
      ctx.fillStyle = 'green';
      this.snake.forEach(segment => {
        ctx.fillRect(segment.x, segment.y, this.gridSize, this.gridSize); // Dibuja cada segmento de la serpiente
      });
    },
    drawFood() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
      ctx.fillStyle = 'red';
      ctx.fillRect(this.food.x, this.food.y, this.gridSize, this.gridSize); // Dibuja la comida
    },
    updateSnakePosition() {
      const head = { x: this.snake[0].x + this.direction.x * this.gridSize, y: this.snake[0].y + this.direction.y * this.gridSize }; // Nueva cabeza de la serpiente
      this.snake.unshift(head); // Agrega la nueva cabeza al inicio

      // Verifica si la serpiente ha comido la comida
      if (head.x === this.food.x && head.y === this.food.y) {
        this.score++; // Incrementa la puntuación
        this.changeFoodPosition(); // Cambia la posición de la comida
      } else {
        this.snake.pop(); // Elimina el último segmento si no ha comido
      }

      // Verifica colisiones con los bordes
      if (head.x < 0 || head.x >= this.$refs.gameCanvas.width || head.y < 0 || head.y >= this.$refs.gameCanvas.height) {
        this.resetGame(); // Reinicia el juego si colisiona
      }

      // Verifica colisiones con el cuerpo de la serpiente
      for (let i = 1; i < this.snake.length; i++) {
        if (head.x === this.snake[i].x && head.y === this.snake[i].y) {
          this.resetGame(); // Reinicia el juego si colisiona con su propio cuerpo
        }
      }
    },
    changeFoodPosition() {
      const canvas = this.$refs.gameCanvas; // Obtiene el canvas
      this.food.x = Math.floor(Math.random() * (canvas.width / this.gridSize)) * this.gridSize; // Nueva posición X
      this.food.y = Math.floor(Math.random() * (canvas.height / this.gridSize)) * this.gridSize; // Nueva posición Y
    },
    changeDirection(event) {
      switch (event.key) {
        case 'ArrowUp':
          if (this.direction.y === 0) this.direction = { x: 0, y: -1 }; // Cambia a arriba
          break;
        case 'ArrowDown':
          if (this.direction.y === 0) this.direction = { x: 0, y: 1 }; // Cambia a abajo
          break;
        case 'ArrowLeft':
          if (this.direction.x === 0) this.direction = { x: -1, y: 0 }; // Cambia a izquierda
          break;
        case 'ArrowRight':
          if (this.direction.x === 0) this.direction = { x: 1, y: 0 }; // Cambia a derecha
          break;
      }
    },
    resetGame() {
      this.score = 0; // Reinicia la puntuación
      this.snake = [{ x: 200, y: 200 }]; // Reinicia la serpiente
      this.direction = { x: 0, y: 0 }; // Reinicia la dirección
      this.changeFoodPosition(); // Cambia la posición de la comida
    },
    startAnimation() {
      const animate = () => {
        const canvas = this.$refs.gameCanvas; // Obtiene el canvas
        const ctx = canvas.getContext('2d'); // Obtiene el contexto 2D del canvas
        ctx.clearRect(0, 0, canvas.width, canvas.height); // Limpia el canvas
        this.drawSnake(); // Dibuja la serpiente
        this.drawFood(); // Dibuja la comida
        this.updateSnakePosition(); // Actualiza la posición de la serpiente
        this.animationFrameId = setTimeout(animate, this.speed); // Controla la velocidad de actualización
      };
      animate(); // Inicia la animación
    },
    toggleTutorial() {
      this.showTutorial = !this.showTutorial; // Alterna la visibilidad del tutorial
      this.isGameVisible = false; // Oculta el juego al mostrar el tutorial
    },
    showGame() {
      this.isGameVisible = true; // Muestra el juego
      this.showTutorial = false; // Oculta el tutorial
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
.tutorial-button {
  margin-left: 10px;
  padding: 5px 10px;
  font-size: 14px;
  color: #fff;
  background-color: #ff5722; /* Color del botón de descripción */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}
.tutorial-button:hover {
  background-color: #e64a19; /* Color del botón al pasar el mouse */
}
.title-button {
  background-color: #6200ea; /* Color del botón del título */
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  transition: background-color 0.3s;
}
.title-button:hover {
  background-color: #3700b3; /* Color del botón del título al pasar el mouse */
}
</style> 