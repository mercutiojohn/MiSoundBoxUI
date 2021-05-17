<template>
  <div
    class="television"
    @click="play"
    @mouseenter="ctrlDisplay = true"
    @mouseleave="ctrlDisplay = false"
  >
    <!-- <VideoPlayer
      :options="videoOptions"
      :key="timer"
      @sendPlayerItem="updatePlayerItem"
    /> -->
    <div :class="{ poster: true, 'poster-hided': playing }">
      <img
        :src="
          require('@/assets/img/video-posters/' +
            flvUrls[currUrl].poster +
            '.png')
        "
        alt=""
      />
      <div class="play-button">
        <i class="iconfont icon-play"></i>
      </div>
    </div>
    <video
      id="videoElement"
      controls
      :class="{ 'video-element-hided': !playing }"
    ></video>
    <!-- <div :class="{ controls: true, 'controls-hided': !ctrlDisplay }">
      <button @click="play" v-if="!playing">播放</button>
      <button @click="pause">停止</button>
    </div> -->
  </div>
</template>

<script>
import flvjs from "flv.js";

import VideoPlayer from "@/components/VideoPlayer";
export default {
  name: "Television",
  components: { VideoPlayer },
  data() {
    return {
      timer: "",
      videooObj: null,
      currUrl: 0,
      flvUrls: [
        {
          title: "Spinnin Records Live",
          url:
            "https://flv67a4fdc7.live.126.net/live/77b43eec6d994802928c13746a45d909.flv?netease=flv67a4fdc7.live.126.net",
          poster: "spinnin",
        },
      ],
      playing: false,
      ctrlDisplay: false,
      flvPlayer: null,
      // videoOptions: {
      //   flvjs: {
      //     mediaDataSource: {
      //       isLive: false,
      //       cors: true,
      //       withCredentials: false,
      //     },
      //   },
      //   autoplay: false,
      //   controls: true,
      //   sources: [
      //     {
      //       src:
      //         "https://flv67a4fdc7.live.126.net/live/77b43eec6d994802928c13746a45d909.flv?netease=flv67a4fdc7.live.126.net",
      //       type: "video/flv",
      //     },
      //   ],
      // },
    };
  },
  computed: {},
  watch: {},
  methods: {
    updatePlayerItem(playerItem) {
      this.videoObj = playerItem;
    },
    loadVideo() {
      if (flvjs.isSupported()) {
        var videoElement = document.getElementById("videoElement");
        this.flvPlayer = flvjs.createPlayer({
          type: "flv",
          isLive: true,
          hasAudio: true,
          url: this.flvUrls[this.currUrl].url,
        });
        this.flvPlayer.attachMediaElement(videoElement);
        this.flvPlayer.load();
      }
    },
    unloadVideo() {
      this.flvPlayer.pause();
      this.flvPlayer.unload();
      this.flvPlayer.detachMediaElement();
      this.flvPlayer.destroy();
      this.flvPlayer = null;
    },
    play() {
      this.playing = true;
      if (this.flvPlayer == null) this.loadVideo();
      this.flvPlayer.play();
    },
    // pause() {
    //   this.playing = false;
    //   this.flvPlayer.pause();
    // },
  },
  created() {},
  mounted() {
    // this.loadVideo();
  },
  beforeDestroy() {
    this.unloadVideo();
  },
};
</script>

<style>
.television {
  height: 100%;
  width: 100%;
}
#videoElement {
  width: 100%;
  height: 100%;
}
.television .poster {
  width: 100%;
  transition: opacity 0.2s ease;
  cursor: pointer;
  transition: filter 0.2s ease;
}

.television .poster img {
  filter: brightness(60%);
  width: 100%;
}
.television .poster img:active {
  filter: brightness(100%);
}
.television .poster-hided {
  opacity: 0;
  height: 0;
}
.television .controls {
  height: 40px;
  width: 100%;
  background: transparent;
  transition: height 0.2s ease;
  display: flex;
}
.television .controls-hided {
  height: 0;
}
.video-element-hided {
  display: none;
}
.play-button {
  position:absolute;
  top:calc(50% - 40px);
  left:calc(50% - 40px);
  background-color: var(--elem-color);
  color:var(--main-color);
  border-radius: 40px;
  width:80px;
  height:80px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.play-button i{
  font-size: 30px;
}
</style>