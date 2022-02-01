<template>
  <div id="app">
    <h1>Test technique 2 de HeyTeam</h1>
    <CardContainer/>
  </div>
</template>

<script>
import CardContainer from './components/CardContainer.vue'

export default {
  name: 'App',
  components: {
    CardContainer
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
