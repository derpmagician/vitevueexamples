<template>
  <div class="game-view">
    <h1>Selecciona un Juego</h1>
    <div class="button-container">
      <button @click="selectGame('pointCatching')" class="game-button">Juego de Caza de Puntos</button>
      <button @click="selectGame('freeDrawing')" class="game-button">Juego de Dibujo Libre</button>
      <button @click="selectGame('pong')" class="game-button">Juego de Pong</button>
      <button @click="selectGame('snake')" class="game-button">Juego de Snake</button>
    </div>

    <component :is="currentGame" />
    <div class="description">
      <h3>Descripción y Controles</h3>
      <p>{{ gameDescription }}</p>
    </div>
  </div>
</template>

<script>
import PointCatchingGame from '@/components/PointCatchingGame.vue'; // Importa el juego de caza de puntos
import FreeDrawingGame from '@/components/FreeDrawingGame.vue'; // Importa el juego de dibujo libre
import PongGame from '@/components/PongGame.vue'; // Importa el juego de Pong
import SnakeGame from '@/components/SnakeGame.vue'; // Importa el juego de Snake

export default {
  name: 'GameView',
  components: {
    PointCatchingGame, // Registra el componente del juego de caza de puntos
    FreeDrawingGame, // Registra el componente del juego de dibujo libre
    PongGame, // Registra el componente del juego de Pong
    SnakeGame, // Registra el componente del juego de Snake
  },
  data() {
    return {
      currentGame: 'PointCatchingGame', // Juego por defecto
      gameDescription: '', // Descripción del juego actual
    };
  },
  methods: {
    selectGame(game) {
      this.currentGame = game === 'pointCatching' ? 'PointCatchingGame' : 
                         game === 'freeDrawing' ? 'FreeDrawingGame' : 
                         game === 'pong' ? 'PongGame' : 
                         'SnakeGame'; // Cambia el juego actual
      this.updateDescription(); // Actualiza la descripción al seleccionar un juego
    },
    updateDescription() {
      switch (this.currentGame) {
        case 'PointCatchingGame':
          this.gameDescription = 'Objetivo: Atrapa los puntos que aparecen en el canvas. Controles: Haz clic en los puntos para atraparlos.';
          break;
        case 'FreeDrawingGame':
          this.gameDescription = 'Objetivo: Dibuja libremente en el canvas. Controles: Usa el mouse para dibujar.';
          break;
        case 'PongGame':
          this.gameDescription = 'Objetivo: Mantén la pelota en juego y evita que pase tu paleta. Controles: Mueve el mouse para controlar tu paleta.';
          break;
        case 'SnakeGame':
          this.gameDescription = 'Objetivo: Come la comida y crece sin chocar contigo mismo ni con los bordes. Controles: Usa las flechas del teclado para mover la serpiente.';
          break;
        default:
          this.gameDescription = '';
      }
    },
  },
  mounted() {
    this.updateDescription(); // Establece la descripción inicial
  },
};
</script>

<style scoped>
.game-view {
  text-align: center;
}

.button-container {
  display: flex;
  justify-content: center; /* Centra los botones horizontalmente */
  flex-wrap: wrap; /* Permite que los botones se envuelvan si no hay suficiente espacio */
  margin-bottom: 20px;
}

.game-button {
  margin: 5px;
  padding: 10px 20px;
  font-size: 18px;
  color: #fff;
  background-color: #6200ea;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.game-button:hover {
  background-color: #3700b3;
}

.description {
  margin-top: 20px;
  font-size: 16px;
  text-align: left;
}
</style> 