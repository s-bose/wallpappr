### A desktop wallpaper engine for http://reddit.com/r/wallpapers 

![icon](./build/icons/icon.ico)

---
`WallpappR` is a desktop application made using `ElectronJS` and `VueJS` and the Reddit web API to fetch 
wallpapers from the subreddit *r/wallpapers*, no Login required.

Users can download, or set wallpapers directly from the app. 
Users can also change the download directory.
Additionally, links to the original posts can be opened from the app.

---
##### To compile and run development server

```
npm run electron:serve 
```


##### To build the installer in /dist_electron
```
npm run electron:build
```

It is recommended to use `npm update` after the initial `npm install` to resolve Babbel errors.

Youtube demo: https://www.youtube.com/watch?v=JNU3o8XMJS4


### Setup File (Windows & Mac)
https://sbose.itch.io/wallpappr

for Linux, you can build your own executable by following the build process.
