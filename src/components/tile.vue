<template>
  <div class="container-fluid">
    <div class="justify-content-center d-flex flex-wrap">
      <div class="mb-1 pics" v-for="post in postsWithPreview" :key="post.data.id">
        <div class="card" @click="showLightbox(post)">
          <img :src="post.data.preview.images[0].resolutions[2].url" class="img-fluid" loading="lazy" alt="">
          <div class="content-overlay"></div>
          <div class="content">
            <p>U/{{ post.data.author }}</p>
            <span @click.stop><a target="_blank" v-bind:href=" 'http://www.reddit.com' + post.data.permalink"><i
                  class="fas fa-globe fa-lg" style="color: white;"></i></a></span>
          </div>
        </div>
        <hr class="my-2 style2">
      </div>
    </div>
    <div class="lightbox">
      <div>
        <img src="" alt="">
        <div class="figcaption">
          <div id="info">
            <a target="_blank" href=""></a>
            <p></p>
          </div>
          <div class="icons">
            <a href="#"><i class="fa fa-heart-o"></i></a>
            <a href="#" @click.prevent="saveOptions"><i class="fa fa-arrow-down"></i></a>
            <a href="#"><i class="fa fa-paint-roller"></i></a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script defer>

import axios from 'axios'

// const { remote } = require('electron')
// const { dialog } = remote 
// const path = require('path')
import {ipcRenderer} from 'electron'

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
      currentLightbox: null,
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
      const baseURL = "https://www.reddit.com/r/wallpapers.json?limit=20&count=20"

      var url = baseURL
      if (page != null) {
        url = baseURL + '&after=' + page
      }

      axios.get(url)
         .then(response => {

        var postObjList = response.data.data.children
        
        //filtering out the posts with no preview image
        postObjList = postObjList.filter(function (post) {
           return post.data.preview 
           })


        this.next = response.data.data.after
        this.postsLoading = false
        
        //appending new GET response to the array
        this.postsWithPreview = this.postsWithPreview.concat(postObjList)


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

    async saveOptions() {

      var currentLightbox = this.currentLightbox
      var title = currentLightbox.data.title.replace(/\s/g, '_')
      console.log(title)
      // eslint-disable-next-line no-unused-vars
      // const filePath = await dialog.showSaveDialog({
      //   buttonLabel: 'Save image',
      //   // defaultPath: ``
      // })


      var URL = currentLightbox.data.preview.images[0].source.url
      

      console.log(URL)
      // const Path = path.resolve('images/',  `${title}.jpg`)
      // console.log(Path)
      // const writer = fs.createWriteStream(Path)


      // axios({
      //   url:URL,
      //   method: 'GET',
      //   responseType: 'stream'
      // })
      // .then((response) => {
      //   response.data.pipe(writer)
      // })
      ipcRenderer.send('download', {
        url: URL,
        title: title,
        // directory: 
      })
      ipcRenderer.on("download complete", (event, file) => {
        console.log(file); // Full file path
      });

    },


    showLightbox(post) {
      this.currentLightbox = post
      var previewList = post.data.preview
      var lightbox = document.querySelector('.lightbox')
      var image = lightbox.getElementsByTagName('img')[0]
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
        lightbox.getElementsByTagName('img')[0].src = "" /*reset src*/
        this.currentLightbox = null
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped src="@/components/tile.scss" lang="scss">

</style>