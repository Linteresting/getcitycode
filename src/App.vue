<template>
  <GetCityCode msg="time to get CITYCODE!"/>
</template>

<script>
import GetCityCode from './components/GetCityCode.vue'

export default {
  name: 'App',
  components: {
    GetCityCode
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
