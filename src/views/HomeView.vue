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
  <div class="home">
    <h1>Bienvenido a la Aplicación Vue</h1>
    <p>Esta es una aplicación de ejemplo utilizando Vue y Vite.</p>
  </div>
</template>

<style scoped>
.home {
  text-align: center;
  margin-top: 20px;
}
</style>
