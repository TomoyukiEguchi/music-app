<template>
  <div id="app">
    <FrameiPhone />
  </div>
</template>

<script>
import FrameiPhone from './components/FrameiPhone.vue'

export default {
  name: 'App',
  components: {
    FrameiPhone
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
