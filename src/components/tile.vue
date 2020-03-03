<template>
  <div class="container-fluid">
    <ul class="gallery justify-content-center list-group-flush">
      <li class="mb-1 pics" v-for="post in postsWithPreview" v-bind:key="post.data.id">
        <div class="card" @click="showModal(post)">
          <img :src="post.data.preview.images[0].resolutions[2].url" class="img-fluid" alt="">
          <div class="content-overlay"></div>
          <div class="content">
            <p>U/{{ post.data.author }}</p>
            <span @click.stop><a target="_blank" v-bind:href=" 'http://www.reddit.com' + post.data.permalink"><i class="fas fa-globe fa-lg"
                style="color: white;"></i></a></span>
          </div>
        </div>
        <hr class="my-2 style2">
      </li>
    </ul>
    <imageModal ref="modal"></imageModal>
  </div>
</template>

// https://preview.redd.it/0v22at12k9k41.png?width=320&crop=smart&amp;auto=webp&amp;s=d80c2ddca1f4d6563e18e8f4229fe21a2d5dce95

<script defer>

  import 'bootstrap/dist/css/bootstrap.min.css'

  import imageModal from '@/components/imageModal.vue'

  const axios = require('axios')
  // const jquery = require('jquery')

  const baseURL = "https://www.reddit.com/r/wallpapers.json"
  const limitBy = '?limit=25' //15 by default for faster loading


  export default {
    name: 'tile',
    props: {
      msg: String
    },
    data() {
      return {
        postsWithPreview: []
      }
    },
    components: {
      imageModal,
    },
    created() {
      axios.get(baseURL + limitBy)
        .then(response => {
          
          var postObjList = response.data.data.children
          this.postsWithPreview = postObjList.filter(function(post) {
            return post.data.preview
          })

          function removeAmp(url) {    // ** replacing all occurence of '&amp;' with '&'
            const parseResult = new DOMParser().parseFromString(url, "text/html");
            const parsedURL = parseResult.documentElement.textContent;
            // console.log(parsedURL);
            return parsedURL;
          }
          this.postsWithPreview.forEach(function(post) {
            post.data.preview.images[0].resolutions.forEach(function(res) {
              res.url = removeAmp(res.url);
            })

            post.data.preview.images[0].source.url = removeAmp(post.data.preview.images[0].source.url);
          })
          console.log(this.postsWithPreview);
        })
    },
    computed: {

    },
    methods: {
      
      changeModalContent() {
      },
      showModal(post) {

        console.log(post);
        this.$refs.modal.open(post);
      },
      closeModal() {
        this.$refs.modal.close();
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
    padding: .1em;
    height: auto;
    width: 100%;
    //border-radius: .3em;
    //display: block;
  }

  // ? for the hover effect
  // ? https://codepen.io/ArnaudBalland/pen/vGZKLr?editors=1100

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
    // border-radius: .3em;
    position: relative;
    // overflow: auto;
    box-shadow:
      -7px 7px 12px rgba(255, 255, 255, .5),
      7px 7px 12px rgba(0, 0, 0, .3);
  }

  hr.style2 {
    //display: none;
    opacity: 0;
  }

  .pics .card:hover {
    // margin: 2px;
    // padding: 2px;
    // border-radius: .3em;
    filter: grayscale(70%);
    transform: scale(1.1);
    cursor: zoom-in;
    box-shadow:
      -7px 7px 12px rgba(255, 255, 255, .3),
      7px 7px 12px rgba(0, 0, 0, .3);

  }

  .card .content-overlay {
    // border-radius: .3em;
    position: absolute;
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