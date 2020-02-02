<template>
<!-- Lorem ipsum dolor, sit amet consectetur adipisicing elit. Maiores accusamus alias magni aperiam, temporibus adipisci minima quo magnam tenetur iusto eos totam mollitia aliquid omnis quam nam aut deserunt sequi. -->
  <div class="container-fluid">
    <ul class="gallery justify-content-center list-group-flush">
      <li class="mb-1 pics" v-for="post in postsWithPreview" v-bind:key="post.data.id">
        <div class="card">
          <img :src="post.data.preview.images[0].source.url.replace('amp;s', 's')" class="img-fluid" alt="">
          <div class="content-overlay"></div>
          <div class="content">
            <p>U/{{ post.data.author }}</p>
            <a target="_blank" v-bind:href=" 'http://www.reddit.com' +post.data.permalink"><i class="fas fa-globe fa-lg"
                style="color: white;"></i></a>
          </div>
        </div>
        <hr class="my-2 style2">
      </li>
    </ul>

  </div>



</template>

<script>
  import 'bootstrap'
  import 'bootstrap/dist/css/bootstrap.min.css'



  const axios = require('axios')


  const baseURL = "https://www.reddit.com/r/wallpapers.json"
  export default {
    name: 'tile',
    props: {
      msg: String
    },
    data() {
      return {
        showModal: false,
        postObjList: []
      }
    },
    components: {

    },
    created() {
      axios.get(baseURL)
        .then(response => {
          this.postObjList = response.data.data.children
          console.log(this.postObjList)
          console.log(this.postsWithPreview)
        })
    },
    computed: {
      postsWithPreview: function() {
        return this.postObjList.filter(function(post) {
          return post.data.preview
        })
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  @import url('https://fonts.googleapis.com/css?family=Quicksand&display=swap');

  .gallery {
    padding-left: 15%;
    padding-right: 15%;
  }

  * {
    font-family: 'Quicksand';
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    padding: 5px;
  }

  hr.style2 {
    border: 0;
    height: 1px;
    background-image: linear-gradient(to right, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.2), rgba(0, 0, 0, 0));
  }

  img {
    //padding: .2em;
    height: auto;
    width: 100%;
    //border-radius: .5em;
    //display: block;
  }

  //for the hover effect
  //https://codepen.io/ArnaudBalland/pen/vGZKLr?editors=1100

  .gallery {
    column-count: 3;
    column-width: 33.3333333%;
  }

  @media (max-width: 900px) {
    .gallery {
      -webkit-column-count: 2;
      -moz-column-count: 2;
      column-count: 2;
      column-width: 50%;
    }
  }

  @media (max-width: 650px) {
    .gallery {
      -webkit-column-count: 1;
      -moz-column-count: 1;
      column-count: 1;
      column-width: 100%;
    }
  }

  .card {
    //border-radius: .5em;
    
  }

  hr.style2 {
    //display: none;
    opacity: 0;
  }

  .pics .card:hover {
    // margin: 2px;
    // padding: 2px;
    //transform: scale(1.05);
    cursor: zoom-in;
    box-shadow:
      -7px 7px 12px rgba(255, 255, 255, .3),
      7px 7px 12px rgba(0, 0, 0, .3);

  }

  .card .content-overlay {
    //border-radius: .4em;
    background: linear-gradient(to bottom, rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.6));
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

  .card:hover .content-overlay {
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