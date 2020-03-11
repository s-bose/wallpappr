<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <tile/>
    <!-- <modalEnlarge/> -->
  </div>
</template>

<script>
// @ is an alias to /src
import tile from '@/components/tile.vue'
export default {
  name: 'home',
  components: {
    tile,
  }
}
</script>

<style>

::-webkit-scrollbar {
    display: none;
}

body {
  background: #ffffff;
}
</style>
