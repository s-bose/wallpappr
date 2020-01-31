<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <tile msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import tile from '@/components/tile.vue'

export default {
  name: 'home',
  components: {
    tile
  }
}
</script>

<style>
body {
  background: #eeeeee;
}
</style>
