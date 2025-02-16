<script>
import HomeView from './views/HomeView.vue';
import GameView from './views/GameView.vue';

export default {
  components: {
    HomeView,
    GameView,
  },
  data() {
    return {
      inGame: false,
      isDarkMode: true, // Cambia a false para modo claro
    };
  },
  methods: {
    startGame() {
      this.inGame = true;
    },
    endGame() {
      this.inGame = false;
    },
  },
};
</script>

<template>
  <div id="app" :class="{ 'dark-mode': isDarkMode }">
    <HomeView v-if="!inGame" @startGame="startGame" />
    <GameView v-else @endGame="endGame" />
  </div>
</template>

<style>
body {
  margin: 0;
  font-family: 'Arial', sans-serif;
  background-color: #f0f0f0;
  transition: background-color 0.3s, color 0.3s;
}

.dark-mode {
  background-color: #121212;
  color: #ffffff;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
