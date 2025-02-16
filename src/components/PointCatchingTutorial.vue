<template>
  <div class="tutorial-container">
    <h3>Cómo se desarrolló el Juego de Caza de Puntos</h3>
    <p>Este juego fue desarrollado utilizando Vue.js y HTML5 Canvas. A continuación, se describen los pasos clave en el desarrollo:</p>
    
    <h4>1. Configuración del Canvas</h4>
    <p>Se creó un canvas donde se dibujarán los elementos del juego:</p>
    <pre><code>&lt;canvas ref="gameCanvas" width="600" height="600"&gt;&lt;/canvas&gt;</code></pre>
    <p>El canvas es el área donde se dibujan todos los elementos del juego, como el punto rojo y el arco azul que sigue al mouse.</p>

    <h4>2. Dibujo del Punto</h4>
    <p>Se implementó la lógica para dibujar un punto rojo en el canvas:</p>
    <pre><code>
drawPoint() {
  const canvas = this.$refs.gameCanvas;
  const ctx = canvas.getContext('2d');
  ctx.clearRect(0, 0, canvas.width, canvas.height); // Limpia el canvas
  ctx.fillStyle = 'red'; // Establece el color del punto
  ctx.beginPath();
  ctx.arc(this.point.x, this.point.y, 15, 0, Math.PI * 2); // Dibuja un círculo rojo
  ctx.fill();
}
    </code></pre>
    <p>Esta función se encarga de dibujar el punto rojo en una posición aleatoria en el canvas. Se utiliza <code>clearRect</code> para limpiar el canvas antes de dibujar el nuevo punto.</p>

    <h4>3. Dibujo del Arco Azul</h4>
    <p>Se implementó la lógica para dibujar un arco azul que sigue al mouse:</p>
    <pre><code>
drawArc() {
  ctx.strokeStyle = 'yellow'; // Color del arco
  ctx.lineWidth = 2; // Grosor del arco
  ctx.beginPath();
  ctx.arc(this.mousePosition.x, this.mousePosition.y, this.arcSize, 0, Math.PI * 2); // Dibuja un círculo azul
  ctx.stroke();
}
    </code></pre>
    <p>Esta función dibuja un arco azul que sigue la posición del mouse, lo que permite al jugador interactuar con el punto rojo.</p>

    <h4>4. Interacción del Usuario</h4>
    <p>Se agregó la funcionalidad para que el usuario pueda hacer clic en el punto y aumentar su puntuación:</p>
    <pre><code>
catchPoint(event) {
  const canvas = this.$refs.gameCanvas;
  const rect = canvas.getBoundingClientRect();
  const x = event.clientX - rect.left; // Coordenada X del clic
  const y = event.clientY - rect.top; // Coordenada Y del clic

  const dx = this.mousePosition.x - this.point.x; // Diferencia en X
  const dy = this.mousePosition.y - this.point.y; // Diferencia en Y
  if (Math.sqrt(dx * dx + dy * dy) < 15 + this.arcSize) { // Verifica si el arco azul toca el punto rojo
    this.score++; // Incrementa la puntuación
    this.updateVelocity(); // Aumenta la velocidad si es necesario
    this.changePointPosition(); // Cambia la posición y dirección del punto
    this.drawPoint(); // Dibuja el nuevo punto
  }
}
    </code></pre>
    <p>Esta función verifica si el arco azul toca el punto rojo. Si es así, incrementa la puntuación y cambia la posición del punto.</p>

    <h4>5. Animación del Juego</h4>
    <p>Se implementó un bucle de animación para actualizar la posición del punto y redibujar el canvas:</p>
    <pre><code>
startAnimation() {
  this.changePointPosition(); // Genera posición aleatoria al inicio

  const animate = () => {
    this.updatePosition(); // Actualiza la posición del punto
    this.drawPoint(); // Dibuja el punto y el arco
    this.animationFrameId = requestAnimationFrame(animate); // Solicita el siguiente frame de animación
  };
  animate(); // Inicia la animación
}
    </code></pre>
    <p>Esta función inicia el bucle de animación que actualiza la posición del punto y redibuja el canvas en cada frame.</p>

    <h4>6. Actualización de la Velocidad</h4>
    <p>Se implementó la lógica para aumentar la velocidad del punto cada 10 puntos:</p>
    <pre><code>
updateVelocity() {
  if (this.score % 10 === 0) { // Cada 10 puntos
    this.velocity.x *= 1.1; // Aumenta la velocidad en el eje X
    this.velocity.y *= 1.1; // Aumenta la velocidad en el eje Y
  }
}
    </code></pre>
    <p>Esta función aumenta la velocidad del punto cada vez que el jugador alcanza un múltiplo de 10 en la puntuación.</p>
  </div>
</template>

<script>
export default {
  name: 'PointCatchingTutorial', // Nombre del componente
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
  color: rgb(219, 178, 44);
  padding: 10px;
  border-radius: 5px;
}
</style> 