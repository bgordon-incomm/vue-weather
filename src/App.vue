<template>
  <div id="app">

    <forecast-component />
  </div>
</template>

<script>
import ForecastComponent from '@/components/ForecastComponent.vue'

export default {
  name: 'app',
  components: {
    ForecastComponent
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
