<script>
export default {
  name: 'App',
  data() {
    return {
      isDarkMode: false, // Estado del modo oscuro
    };
  },
  methods: {
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode; // Alterna el estado del modo oscuro
      console.log('Modo oscuro:', this.isDarkMode); // Verifica el estado
    },
  },
}
</script>

<template>
  <div id="app" :class="{ 'dark-mode': isDarkMode }">
    <h1>Bienvenido a mi SPA con Vue y Vite</h1>
    <button @click="toggleDarkMode">Toggle Dark Mode</button>
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
