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
            <a href="#" @click="changeDirectory"><i class="far fa-folder"></i></a>
            <a href="#" @click="saveWallpaper"><i class="fa fa-arrow-down"></i></a>
            <a href="#" @click="setWallpaper"><i class="fa fa-paint-roller"></i></a>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>


<script defer>

import axios from 'axios'
import { ipcRenderer } from 'electron'
const wallpaper = require('wallpaper')
const { dialog } = require('electron').remote

export default {
  name: 'tile',
  props: {

  },
  data() {
    return {
      postsWithPreview: [],
      postsLoading: false,
      next: null,
      currentLightbox: null,
      path: null,
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
      const baseURL = "https://www.reddit.com/r/wallpapers.json?limit=15&count=15"

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
      let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement
        .offsetHeight;
      if (bottomOfWindow) {
        console.log("End reached")
        if (this.next != null) {
          this.getPosts(this.next)
        }
      }

    },


    async saveImage( /*callback function (optional)*/ setimg) {
      var currentLightbox = this.currentLightbox
      var title = currentLightbox.data.title.replace(/\s/g, '_')
      console.log(title)

      var URL = currentLightbox.data.preview.images[0].source.url


      console.log(URL)

      ipcRenderer.send('download', {
        url: URL,
        directory: this.path
      })
      
      this.$buefy.toast.open({
        message: 'Downloading...',
        type: 'is-dark',
        position: 'is-bottom'
      })
      
      ipcRenderer.on("download complete", (event, file) => {
        // console.log(file); // Full file path
        if (setimg) {
          setimg(file);
        }
      })
    },

    async setImage(filePath) {
      // console.log(filePath)
      await wallpaper.set(filePath)
    },

    async saveWallpaper() {
      this.saveImage();
    },

    async setWallpaper() {
      this.saveImage(this.setImage);
    },

    async changeDirectory() {
      const filepath = dialog.showOpenDialogSync({
        properties: ['openDirectory', 'createDirectory', 'promptToCreate' ]
      })
      this.path = filepath[0]
      console.log(this.path)
    },

    showLightbox(post) {
      this.currentLightbox = post
      var previewList = post.data.preview
      var lightbox = document.querySelector('.lightbox')
      var image = lightbox.getElementsByTagName('img')[0]
      var imageSrcs = previewList.images[0]
      image.src = imageSrcs.source.url
      // eslint-disable-next-line no-unused-vars
      const loadingComponent = this.$buefy.loading.open({
        canCancel: true,
      })

      image.onload = function () {
        loadingComponent.close()
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