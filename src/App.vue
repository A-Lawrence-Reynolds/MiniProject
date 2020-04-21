<template>
  <div id="app">
    <Engineers/>
    <!-- <Schedule/> -->
  </div>
</template>

<script>
import Engineers from './components/Engineers.vue'

export default {
  name: 'App',
  
  components: {
    Engineers,
    // Schedule

  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 60px;
  font-weight: 900;
}
</style>
