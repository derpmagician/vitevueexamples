<script>
export default {
  name: 'App',
}
</script>

<template>
  <div id="app">
    <h1>Bienvenido a mi SPA con Vue y Vite</h1>
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/game">Juego</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>

<style>
/* Estilos globales */
@import './assets/base.css';
@import './assets/main.css';
</style>
