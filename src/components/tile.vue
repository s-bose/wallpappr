<template>
  <div class="container-fluid">
    <ul class="gallery justify-content-center list-group-flush">
      <li class="mb-1 pics" v-for="post in postsWithPreview" v-bind:key="post.data.id">
        <div class="card" @click="showLightbox(post)">
          <img :src="post.data.preview.images[0].resolutions[2].url" class="img-fluid" alt="">
          <div class="content-overlay"></div>
          <div class="content">
            <p>U/{{ post.data.author }}</p>
            <span @click.stop><a target="_blank" v-bind:href=" 'http://www.reddit.com' + post.data.permalink"><i
                  class="fas fa-globe fa-lg" style="color: white;"></i></a></span>
          </div>
        </div>
        <hr class="my-2 style2">
      </li>
    </ul>
    <div class="lightbox">
      <div>
        <img src="" alt="">
        <div class="figcaption">
          <a target="_blank" href=""><h6></h6></a>
          <div class="icons">
            <span><a href="#"><i class="fa fa-heart-o"></i></a></span>
            <span><a href="#"><i class="fa fa-arrow-down"></i></a></span>
            <span><a href="#"><i class="fa fa-paint-roller"></i></a></span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script defer>

  // import 'bootstrap/dist/css/bootstrap.min.css'


const axios = require('axios')
// const fs = require('fs')
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

  },
  created() {
    axios.get(baseURL + limitBy)
      .then(response => {

        var postObjList = response.data.data.children
        this.postsWithPreview = postObjList.filter(function (post) {
          return post.data.preview
        })

        function removeAmp(url) { // ** replacing all occurence of '&amp;' with '&'
          const parseResult = new DOMParser().parseFromString(url, "text/html");
          const parsedURL = parseResult.documentElement.textContent;
          // console.log(parsedURL);
          return parsedURL;
        }
        this.postsWithPreview.forEach(function (post) {
          post.data.preview.images[0].resolutions.forEach(function (res) {
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
    showLightbox(post) {
      var previewList = post.data.preview
      var lightbox = document.querySelector('.lightbox')
      console.log(post)
      var image = lightbox.getElementsByTagName('img')[0]
      // var div = lightbox.getElementsByTagName('div')[0]
      console.log(image)

      var imageSrcs = previewList.images[0]
      image.src = imageSrcs.source.url

      image.onload = function () {
        lightbox.classList.add('active')
        // lightbox.getElementsByClassName('figcaption')[0].style.width = image.width + 'px'
        lightbox.querySelector('.figcaption > a > h6').textContent = post.data.title
        lightbox.querySelector('.figcaption > a').href = "http://reddit.com" + post.data.permalink
        // lightbox.querySelector('')
      }


      lightbox.addEventListener('click', e => {
        if (e.target !== e.currentTarget) return
        lightbox.classList.remove('active')
        lightbox.getElementsByTagName('img')[0].src = ""
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">@import url('https://fonts.googleapis.com/css?family=Quicksand&display=swap');

.container-fluid {
  padding: 0px !important;
}

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


.lightbox {
  position: fixed;
  // overflow: hidden;
  z-index: 999;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  display: none;
  // opacity: 1;
  // visibility: visible;
  // transition: all .4s linear;
}

.lightbox.active {
  display: flex;
  flex-direction: column;
  // transition: .4s ease-in-out;
  overflow: hidden;
  // opacity: 1;
  // visibility: visible;
  justify-content: center;
  align-items: center;
}


.lightbox>div {
  height: auto;
  width: auto;
  display: flex;
  flex-direction: column;
  // width:  33vw;
  justify-content: center;
}

.lightbox>div .figcaption {
  font-size: max(1.5vw, 10px);
  display: flex;
  text-align: left;
  justify-content: space-between;
  color: white;
  padding-top: 1em;
}


.lightbox>div .figcaption span {
  // position: relative;
  padding-left: 2em;
  color: white;
}

.lightbox>div .figcaption a {
  color: white;
}

.lightbox>div .figcaption span>a:hover {
  color: red;
}


.lightbox img {
  // display: grid;
  max-width: 100vh;
  max-height: 100vh;
  width: auto;
  height: auto;
  // width: 80%;
  // height: 70%;
  padding: .3em;
  background-color: rgba(0, 0, 0, 0.2);
  border: 1px solid white;
}

// ? for the hover effect
// ? https://codepen.io/ArnaudBalland/pen/vGZKLr?editors=1100

.gallery {
  column-count: 3;
  column-width: 33.3333333%;
}

@media (max-width: 970px) {
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