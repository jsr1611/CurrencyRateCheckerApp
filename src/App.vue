<template>
<div> 
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <currencies/>
</div>
</template>

<script>
import Currencies from './components/Currencies.vue'

export default {
  name: 'App',
  components: {
    Currencies
  }
}
// msg="Welcome to Your Vue.js App"
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
