<template>
<div class="container-fluid">
  <ul class="gallery justify-content-center list-group-flush">
    <li class="mb-3 pics" v-for="post in postObjList" v-bind:key="post.data.id">
      <img :src="post.data.preview.images[0].source.url.replace('amp;s', 's')" class="img-fluid" alt="">
    </li>
    <!-- <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail2" src="" alt="">
    </li>
    <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail3" src="" alt="">
    </li>
    <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail4" src="" alt="">
    </li>
    <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail5" src="" alt="">
    </li>
    <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail6" src="" alt="">
    </li>
    <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail7" src="" alt="">
    </li>
    <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail8" src="" alt="">
    </li>
    <li class="mb-3 pics ">
      <img class="img-fluid" id="thumbnail9" src="" alt="">
    </li> -->
  </ul>
</div>
  


</template>

<script>
  import 'bootstrap'
  import 'bootstrap/dist/css/bootstrap.min.css'

  const axios = require('axios')
  //const jquery = require('jquery')


  const baseURL = "https://www.reddit.com/r/wallpapers.json"
  export default {
    name: 'tile',
    props: {
      msg: String
    },
    data() {
      return {
        postObjList: []
      }
    },

    created() {

      // var posts = {
      //   thumbnails: [],
      //   titles: [],
      //   link: []
      // }

      axios.get(baseURL)
        .then(response => {

          this.postObjList = response.data.data.children
          console.log(this.postObjList)



          // postObjList.forEach(element => {
          //   posts.thumbnails.push(element.data.preview.images[0].source.url.replace('amp;s', 's'))
          //   posts.titles.push(element.data.title)
          //   posts.link.push(element.data.url)
          // });

          // console.log(posts)
          // document.getElementById('thumbnail1').src = posts.thumbnails[0];
          // document.getElementById('thumbnail2').src = posts.thumbnails[1];
          // document.getElementById('thumbnail3').src = posts.thumbnails[2];
          // document.getElementById('thumbnail4').src = posts.thumbnails[3];
          // document.getElementById('thumbnail5').src = posts.thumbnails[4];
          // document.getElementById('thumbnail6').src = posts.thumbnails[5];
          // document.getElementById('thumbnail7').src = posts.thumbnails[6];
          // document.getElementById('thumbnail8').src = posts.thumbnails[7];
          // document.getElementById('thumbnail9').src = posts.thumbnails[8];
          
          
          //let posts = thumbnails.replace('amp;s', 's')
          //document.getElementById("thumbnail").src = posts
          //jquery('#test').attr('src', thumbnail)
        })

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    padding: 5px;
  }

  a {
    color: #42b983;
  }

  img {
    //padding: .2em;
    height: auto;
    width: 100%;
    //border-radius: .5em;
    //display: block;
    transition: .3s ease-in-out;
  }

// .container-fluid {
//   background-color:rgba(253, 253, 253, 0.239)
// }


  .gallery {
  column-count: 3;
  column-width: 33%;
  }

 


  @media (max-width: 700px) {
  .gallery {
    -webkit-column-count: 2;
    -moz-column-count: 2;
    column-count: 2;
    column-width: 50%;}
  }

  @media (max-width: 450px) {
  .gallery {
    -webkit-column-count: 1;
    -moz-column-count: 1;
    column-count: 1;
    column-width: 100%;}
  }


  .gallery .pics :hover {
    // margin: 2px;
    // padding: 2px;
    //transform: scale(1.05);
    box-shadow: 
      -7px 7px 12px rgba(255,255,255,.3),
      7px 7px 12px rgba(0, 0, 0, .3)
  }

  
</style>