<template>
  <div id="app"
       @contextmenu.prevent=""
       @dragenter="$event.preventDefault()"
       @dragover="$event.preventDefault()"
       @drop="$event.preventDefault()"
       v-visibility-change="visibilityChange">
    <div id="blur-container" class="blur-container">
      <svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="100%" height="100%" id="blurred_mkvvpnf50"
           class="blured-img" viewBox="0 0 1920 875" preserveAspectRatio="none">
        <filter id="blur_mkvvpnf">
          <feGaussianBlur in="SourceGraphic" stdDeviation="50"></feGaussianBlur>
        </filter>
        <image x="0" y="0" width="100%" height="100%" externalResourcesRequired="true"
               xmlns:xlink="http://www.w3.org/1999/xlink"
               xlink:href="https://static.wildfirechat.cn/web_wfc_bg2.jpeg"
               style="filter:url(#blur_mkvvpnf)" preserveAspectRatio="none"></image>
      </svg>
      <div class="blur-mask"></div>
    </div>
    <!--用来实现视频缩略图-->
    <div id="styled_video_container" class="styled_video_container">
      <video id="bgvid" playsinline autoplay muted loop>
        <!-- <source src="http://thenewcode.com/assets/videos/polina.webm" type="video/webm">
        <source src="http://thenewcode.com/assets/videos/polina.mp4" type="video/mp4"> -->
      </video>
    </div>

    <router-view id="main-content-container" class="main-content-container"></router-view>
  </div>
</template>

<script>
import store from "@/store";
import {isElectron} from "@/platform";

export default {
  name: 'App',
  data() {
    return {
      url: '',
      sharedMiscState: store.state.misc,
    }
  },
  methods: {
    visibilityChange(event, hidden) {
      store.setPageVisibility(hidden);
    }
  },

  created() {
    let root = document.documentElement;
    if (isElectron() || window.location.href.indexOf('voip') >= 0) {
      root.style.setProperty('--main-margin-left', '0px');
      root.style.setProperty('--main-margin-right', '0px');
      root.style.setProperty('--main-margin-top', '0px');
      root.style.setProperty('--main-margin-bottom', '0px');
      root.style.setProperty('--home-menu-padding-top', '60px')
    }
    if (this.sharedMiscState.isElectronWindows) {
      root.style.setProperty('--main-border-radius', '0px')
    }
  },

  mounted() {
    if (window.location.href.indexOf('voip') >= 0) {
      let app = document.getElementById("app");
      let el = document.getElementById("blur-container");
      app.removeChild(el)
      el = document.getElementById('styled_video_container');
      app.removeChild(el)
      el = document.getElementById('main-content-container');
      el.style.backgroundColor = '#292929'
    }
  }
}

</script>

<!--should not scoped-->
<style lang="css">

:root {
  --main-border-radius: 10px;
  --main-margin-left: 80px;
  --main-margin-right: 80px;
  --main-margin-top: 50px;
  --main-margin-bottom: 50px;
  --tippy-right: 0px;
  --home-menu-padding-top: 20px;
}

.tippy-tooltip {
  right: var(--tippy-right) !important;
  border: 1px solid #f5f5f5 !important;
  background-color: #fcfcfc !important;
  box-shadow: 0 0 25px rgba(0, 0, 0, 0.125);
}

#app {
  background-color: red;
  position: relative;
}

.blur-container {
  overflow: hidden;
  height: 100vh;
  width: 100vw;
  z-index: -10;
  position: fixed;
  margin: 0;
}

.blur-container .blur-mask {
  position: absolute;
  left: 0;
  top: 0;
  height: 100vh;
  width: 100vw;
  background: rgba(0, 0, 0, .2);
  overflow: hidden;
}

.styled_video_container {
  position: fixed;
  top: 0;
  left: 0;
  width: auto;
  height: auto;
  z-index: -999;
  background-size: cover;
  transition: 1s opacity;
}

.main-content-container {
  z-index: 999;
  position: absolute;
  width: calc(100vw - var(--main-margin-left) - var(--main-margin-right));
  height: calc(100vh - var(--main-margin-top) - var(--main-margin-bottom));
  top: 0;
  left: 0;
  margin: var(--main-margin-top) var(--main-margin-right) var(--main-margin-bottom) var(--main-margin-left);
  display: flex;
  justify-content: center;
  align-items: center;
}

.container-emoji {
  height: 300px;
}


</style>
