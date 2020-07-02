<template>
<!-- Entry point of the VueJs application -->
  <div id="app">
  Hello World
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
      return {
        api_key: process.env.VUE_APP_API_KEY,
      }
  }
}
</script>

<style>
#app {
 
}
</style>
