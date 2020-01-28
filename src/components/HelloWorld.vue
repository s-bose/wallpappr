<template>
  <div class="hello">
    <img id= "test" src="" alt="">
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
    axios.get('https://www.reddit.com/r/wallpapers.json?raw_json=1')
    .then(response => {
      let postObjList = response.data.data.children
      console.log(postObjList)
      let thumbnail = postObjList[0].data.preview.images[0].source.url
      console.log(thumbnail)
      //thumbnail = thumbnail.replace('amp;s', 's')
      //document.getElementById("test").src = thumbnail
      jquery('#test').attr('src', thumbnail)
    })
  }
}




</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
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

img {

  height: 30%;
  width: 30%;
  border-radius: .5em;
  box-shadow: 
    -12px -12px 12px 0 rgba(255, 255, 255, 1),
    8px 8px 12px 0 rgba(0,0,0,.3);
  
}

</style>
