<template>
  <div>
    <h1>Selecciona un Juego</h1>
    <button @click="selectGame('pointCatching')">Juego de Caza de Puntos</button>
    <button @click="selectGame('freeDrawing')">Juego de Dibujo Libre</button>
    <button @click="selectGame('pong')">Juego de Pong</button>
    <button @click="selectGame('snake')">Juego de Snake</button>

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
.description {
  margin-top: 20px;
  font-size: 16px;
  text-align: left;
}
</style> 