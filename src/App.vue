<template>
  <div id="app">
    <Orderlist />
  </div>
</template>

<script>
import Orderlist from './components/orderlist.vue'

export default {
  name: 'App',
  components: {
    Orderlist
  }
}
</script>

<style>
html {
  background-color: rgba(232, 229, 218, .3);
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: rgb(99, 90, 52);
}
</style>
