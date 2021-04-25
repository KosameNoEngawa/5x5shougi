<template>
  <div id="app">
    <Field></Field>
  </div>
</template>

<script>
import Field from './components/field.vue'

export default {
  name: 'App',
  components: {
    Field,
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
  width: 50%;
  margin: 0 auto;
}
</style>
