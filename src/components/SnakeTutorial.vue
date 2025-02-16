<template>
  <div class="tutorial-container">
    <h3>Cómo se desarrolló el Juego de Snake</h3>
    <p>Este juego fue desarrollado utilizando Vue.js y HTML5 Canvas. A continuación, se describen los pasos clave en el desarrollo:</p>
    
    <h4>1. Configuración del Canvas</h4>
    <p>Se creó un canvas donde se dibujarán los elementos del juego:</p>
    <pre><code>&lt;canvas ref="gameCanvas" width="600" height="600"&gt;&lt;/canvas&gt;</code></pre>

    <h4>2. Dibujo de la Serpiente</h4>
    <p>Se implementó la lógica para dibujar la serpiente en el canvas:</p>
    <pre><code>
drawSnake() {
  const canvas = this.$refs.gameCanvas;
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = 'green';
  this.snake.forEach(segment => {
    ctx.fillRect(segment.x, segment.y, this.gridSize, this.gridSize); // Dibuja cada segmento de la serpiente
  });
}
    </code></pre>

    <h4>3. Dibujo de la Comida</h4>
    <p>Se implementó la lógica para dibujar la comida en el canvas:</p>
    <pre><code>
drawFood() {
  const canvas = this.$refs.gameCanvas;
  const ctx = canvas.getContext('2d');
  ctx.fillStyle = 'red';
  ctx.fillRect(this.food.x, this.food.y, this.gridSize, this.gridSize); // Dibuja la comida
}
    </code></pre>

    <h4>4. Movimiento de la Serpiente</h4>
    <p>Se implementó la lógica para actualizar la posición de la serpiente y verificar colisiones:</p>
    <pre><code>
updateSnakePosition() {
  const head = { x: this.snake[0].x + this.direction.x * this.gridSize, y: this.snake[0].y + this.direction.y * this.gridSize };
  this.snake.unshift(head); // Agrega la nueva cabeza al inicio
  // Verifica colisiones y actualiza la puntuación
}
    </code></pre>
  </div>
</template>

<script>
export default {
  name: 'SnakeTutorial',
};
</script>

<style scoped>
.tutorial-container {
  margin-top: 20px;
  text-align: left;
}
h3 {
  margin-bottom: 10px;
}
h4 {
  margin-top: 15px;
}
pre {
  background-color: #09074d;
  color:rgb(219, 178, 44);
  padding: 10px;
  border-radius: 5px;
}
</style> 