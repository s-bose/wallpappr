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
// import modalEnlarge from '@/components/modalEnlarge.vue'
export default {
  name: 'home',
  components: {
    tile,
    // modalEnlarge
  }
}
</script>

<style>
body {
  background: #ffffff;
}
</style>
