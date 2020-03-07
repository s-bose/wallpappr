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
          <div id="info">
            <a target="_blank" href=""></a>
            <p>hello world</p>
          </div>
          <div class="icons">
            <a href="#"><i class="fa fa-heart-o"></i></a>
            <a href="#"><i class="fa fa-arrow-down"></i></a>
            <a href="#"><i class="fa fa-paint-roller"></i></a>
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




export default {
  name: 'tile',
  props: {
    msg: String
  },
  data() {
    return {
      postsWithPreview: [],
      postsLoading: false,
      next: null,
    }
  },
  components: {

  },
  created() {

    this.getPosts()

    window.addEventListener('scroll', this.handleScroll)
     
  },

  methods: {

    getPosts(page) {
      const baseURL = "https://www.reddit.com/r/wallpapers.json?limit=25&count=25"

      var url = baseURL
      if (page != null) {
        url = baseURL + '&after=' + page
      }

      axios.get(url)
         .then(response => {

        var postObjList = response.data.data.children
        this.next = response.data.data.after
        this.postsLoading = false
        this.postsWithPreview = this.postsWithPreview.concat(postObjList.filter(function (post) {
           return post.data.preview 
           }))


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
      .catch(error => {
        console.log(error)
      })

    },

    handleScroll() {
      let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight;
      if (bottomOfWindow) {
        console.log("End reached")
        if (this.next != null) {
          this.getPosts(this.next)
        }
      }
      
    },


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
        var info = document.getElementById('info').childNodes
        info[0].textContent = post.data.title
        info[0].href = "http://reddit.com" + post.data.permalink
        info[1].textContent = 'u/' + post.data.author
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

::-webkit-scrollbar {
    display: none;
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

#info {
  flex-direction: column;
}

#info>p {
  font-size: .7em;
}

.lightbox {
  position: fixed;
  
  z-index: 999;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: none;
  // opacity: 1;
  // visibility: visible;
  // transition: all .4s linear;
}

.lightbox.active {
  overflow-y: scroll;
  display: flex;
  flex-direction: column;
  // transition: .4s ease-in-out;
  // overflow: hidden;
  // opacity: 1;
  // visibility: visible;
  justify-content: center;
  align-items: center;
}


.lightbox>div {
  position: absolute;
  height: auto;
  // width: auto;

  display: flex;
  flex-direction: column;
  width: 50%;
  justify-content: center;
  top: 10%;
  padding-bottom: 2%;
}

.lightbox>div .figcaption {
  width: 100%;
  // font-size: max(1.5em, 10px);
  display: flex;
  text-align: left;
  justify-content: space-between;
  color: white;
  padding-top: 1em;
}


.lightbox>div .figcaption .icons {
  display: flex;
}

.lightbox>div .figcaption .icons a {
  font-size: .7em;
  // position: relative;
  padding-left: 2em;
  color: white;
}


#info a {
  color: white;
  font-size: .8em;
}

#info p {
  font-size: .7em;
}

// .lightbox>div .figcaption a {
  
// }

.lightbox>div .figcaption .icons a:hover {
  color: red;
}


.lightbox img {
  // display: grid;
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  box-shadow: 0px 0px 100px 3px rgba(0,0,0,1);
  margin: 0;
  padding: 0;
  // padding: .3em;
  // background-color: rgba(0, 0, 0, 0.2);
  // border: 1px solid white;
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