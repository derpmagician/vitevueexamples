<script>
export default {
  name: 'HomeView',
  // Consideraciones:
  // 1. Asegúrate de que la etiqueta <base> esté configurada correctamente en index.html
  //    para que las rutas funcionen en GitHub Pages.
  // 2. La propiedad "homepage" en package.json debe coincidir con la URL de tu repositorio.
  // 3. Si utilizas Vue Router, asegúrate de que las rutas sean relativas y estén configuradas
  //    para trabajar con GitHub Pages.
  // 4. Verifica que todos los recursos (imágenes, estilos, etc.) se carguen correctamente
  //    desde la carpeta "public" o que estén bien referenciados.
};
</script>

<template>
  <div class="home-container">
    <h1>Bienvenido a los Juegos</h1>
    <p>Selecciona un juego para comenzar.</p>
    <button @click="$emit('startGame')" class="start-button">Iniciar Juegos</button>
  </div>
</template>

<style scoped>
.home-container {
  text-align: center;
}

.start-button {
  padding: 10px 20px;
  font-size: 18px;
  color: #fff;
  background-color: #6200ea;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.start-button:hover {
  background-color: #3700b3;
}
</style>
