<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </nav>
    <router-view></router-view>
    <ul>
      <li v-for="item in apiData" :key="item.id">{{ item.title }}</li>
    </ul>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      apiData: [] // Initialiser comme un tableau vide
    }
  },
  methods: {
    async fetchData() {
      const response = await fetch('http://localhost:5201/todos')
      this.apiData = await response.json()
    }
  },
  mounted() {
    this.fetchData() // Appeler fetchData lorsque le composant est monté
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>