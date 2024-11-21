<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Edu+AU+VIC+WA+NT+Pre&display=swap');
 *{
  font-family: "Edu AU VIC WA NT Pre", cursive;
  font-optical-sizing: auto;
  font-weight: 400;
  font-style: normal;
  margin: 0;
}
</style>
