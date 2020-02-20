<template>
  <div id="app">
    <div id="nav">
      <!-- <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> -->
    </div>
    <router-view/>

  </div>
</template>

<style scoped>


#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  justify-content: center;
  text-align: center;
  color: #2c3e50;
  background: rgb(200,198,198);
  background: linear-gradient(180deg, rgba(200,198,198,1) 0%, rgba(236,236,236,1) 33%, rgba(255,255,255,1) 100%);
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-size: cover;

}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}



</style>
