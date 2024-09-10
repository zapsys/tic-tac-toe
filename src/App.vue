<template>
  <v-app :theme="theme">
    <v-app-bar class="px-3" location="bottom">
      <v-btn prepend-icon="mdi-github" :href="github_url" target="_blank">GitHub</v-btn>
      <v-spacer></v-spacer>
      <p class="text-center">&copy;{{ new Date().getFullYear() }}</p>
      <v-spacer></v-spacer>
      <v-btn :prepend-icon="theme === 'light' ? 'mdi-weather-sunny' : 'mdi-weather-night'" text="Tema" slim
        @click="toggleTheme">
      </v-btn>
    </v-app-bar>
    <v-main>
      <Board/>
    </v-main>
  </v-app>
</template>

<script setup>
import { ref } from "vue"

const theme = ref('dark')

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
}
</script>
<script>
import Board from './components/Board.vue'

export default {
  name: 'App',

  components: {
    Board
  },

  data: () => ({
    github_url: 'https://github.com/zapsys/tic-tac-toe/'
  }),
}
</script>
