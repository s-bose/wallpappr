### A desktop wallpaper engine for http://reddit.com/r/wallpapers 

![icon](./build/icons/icon.ico)

---
`WallpappR` is a desktop application made using `ElectronJS` and `VueJS` and the Reddit web API to fetch 
wallpapers from the subreddit *r/wallpapers*, no Login required.

Users can download, or set wallpapers directly from the app. 
Users can also change the download directory.
Additionally, links to the original posts can be opened from the app.

---
##### To install npm dependencies
```
npm install
```

##### To compile and run development server

```
npm run electron:serve 
```


##### To build the installer in /dist_electron
```
npm run electron:build
```


Youtube demo: https://www.youtube.com/watch?v=JNU3o8XMJS4


## Setup

### Windows & Mac

Binaries are available from the Releases section.

### Linux (Snap)

[![Get it from the Snap Store](https://snapcraft.io/static/images/badges/en/snap-store-black.svg)](https://snapcraft.io/wallpappr)

Other formats (i.e. AppImage) can be built manually by following the above-mentioned build process.


### Note: Some times after system upgrade the snap package may stop working. In that case, please reinstall the snap package.
