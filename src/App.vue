<template>
  <Duration v-model="iso8601"/>
  <br>
  Снаружи модуля: {{ iso8601 }}
</template>

<script>
import Duration from './components/Duration.vue'

export default {
  name: 'App',
  data: function () {
    return {
      iso8601: 'P2010Y08M11DT15H30M6S',
    }
  },
  components: {
    Duration
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
