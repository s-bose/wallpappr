<template>
<div class="container-fluid">
  <ul class="gallery justify-content-center list-group-flush">
    <li class="mb-3 pics" v-for="post in postObjList" v-bind:key="post.data.id">
      <div class="card">
        <img :src="post.data.preview.images[0].source.url.replace('amp;s', 's')" class="img-fluid" alt="">
        <div class="content-overlay"></div>
        <div class="content">
          <p>U/{{ post.data.author }}</p>
        </div>
      </div>
      
    </li>
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
      axios.get(baseURL)
        .then(response => {

          this.postObjList = response.data.data.children
          console.log(this.postObjList)

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


  img {
    //padding: .2em;
    height: auto;
    width: 100%;
    border-radius: .5em;
    //display: block;
  }

  //for the hover effect
  //https://codepen.io/ArnaudBalland/pen/vGZKLr?editors=1100

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

  .card {
    border-radius: .5em;
  }

  .pics .card:hover {
    // margin: 2px;
    // padding: 2px;
    //transform: scale(1.05);
    cursor: zoom-in;
    box-shadow: 
      -7px 7px 12px rgba(255,255,255,.3),
      7px 7px 12px rgba(0, 0, 0, .3);
    
  }

  .card .content-overlay {
    border-radius: .5em;
    background: rgba(0,0,0,0.5);
    position: absolute;
    align-content: right;
    height: 100%;
    width: 100%;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    opacity: 0;
    -webkit-transition: all 0.4s ease-in-out 0s;
    -moz-transition: all 0.4s ease-in-out 0s;
    transition: all 0.4s ease-in-out 0s;
  }

  .card:hover img {
    filter: blur(2px);
  }

  .card:hover .content-overlay{
    opacity: 1;
  }

  .card .content {
    font-size: x-small;
    color: white;
    letter-spacing: 0.25rem;
    position: absolute;
    text-align: center;
    padding-left: 1em;
    padding-right: 1em;
    width: 100%;
    top: 80%;
    left: 50%;
    opacity: 0;
    -webkit-transform: translate(-50%, -50%);
    -moz-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    -webkit-transition: all 0.3s ease-in-out 0s;
    -moz-transition: all 0.3s ease-in-out 0s;
    transition: all 0.3s ease-in-out 0s;
  }


  .card:hover .content {
    top: 50%;
    left: 50%;
    opacity: 1;
  }

  
</style>