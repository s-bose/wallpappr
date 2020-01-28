<template>
  <div class="hello">
    <img id= "test" width= "640" height="320" src="" alt="">
  </div>
</template>

<script>

const axios = require('axios')
const jquery = require('jquery')
//const rp = require('request-promise')
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  created() {
    axios.get('https://www.reddit.com/r/wallpapers.json')
    .then(response => {
      let postObjList = response.data.data.children
      console.log(postObjList)
      let thumbnail = postObjList[0].data.preview.images[0].source.url
      console.log(thumbnail)
      thumbnail = thumbnail.replace('amp;s', 's')
      //document.getElementById("test").src = thumbnail
      jquery('#test').attr('src', thumbnail)
    })
  }
}




</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
