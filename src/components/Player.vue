<template>
  <div id="player">
    <div class="player-wrap">
      <transition name="fade" type="out-in">
        <div
          class="player-expanded-content"
          ref="content"
          v-if="playerWrapDisplay"
          @click="changeWrapState"
        >
          <div class="media-list">
            <div
              class="media-item homework-card"
              v-for="(item, index) in mediaList"
              :key="index"
              @click="audioSwitch(index)"
            >
              <div class="media-item-image">
                <img
                  class="media-item-image-s"
                  :src="getImage(item.cover)"
                  alt=""
                  srcset=""
                />
              </div>
              <div class="media-item-content">
                <span class="card-title">{{ item.title }}</span>
                <span class="card-details">{{ item.artist }}</span>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <transition name="fade" type="out-in">
        <div class="player-entry" v-if="!playerWrapDisplay">
          <div class="player-cover">
            <!-- <video
            id="myVideo"
            class="player-media video-js vjs-default-skin vjs-big-play-centered"
            x-webkit-airplay="true"
            x5-video-player-fullscreen="true"
            preload="auto"
            playsinline="true"
            webkit-playsinline
            x5-video-player-typ="h5"
          >
            <source :type="audioType" :src="audioSrc" />
          </video> -->
            <audio-player
              @sendPlayerItem="updatePlayerItem"
              :options="videoOptions"
              :key="timer"
            />
            <img
              class="player-cover-image"
              :src="getImage(cover)"
              :alt="audioTitle"
              srcset=""
            />
          </div>
          <div class="player-info" @click="changeWrapState">
            <span class="player-info-title">{{ audioTitle }}</span>
            <span class="player-info-artist">{{ artist }}</span>
          </div>
          <div class="player-control-area">
            <div @click="audioControl" class="player-control-btn-playpause">
              <i
                v-if="audioStatus"
                class="player-control-btn-large iconfont icon-pause1"
              >
              </i>
              <i
                v-else
                class="player-control-btn-large iconfont icon-play1"
              ></i>
            </div>
            <i
              class="player-control-btn-medium iconfont icon-chevron-right"
              @click="audioChange('next')"
            ></i>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import AudioPlayer from "@/components/AudioPlayer";
export default {
  name: "Player",
  components: {
    AudioPlayer,
  },
  data() {
    return {
      listHeight: 0,
      playerWrapDisplay: false,
      audioId: 0,
      cover: "",
      audioTitle: "",
      artist: "",
      audioStatus: "",
      timer: "",
      mediaList: [],
      audioObj: null,
      videoOptions: {
        autoplay: false,
        controls: false,
        sources: [
          {
            src: "",
            type: "",
          },
        ],
      },
    };
  },

  methods: {
    changeWrapState() {
      if (this.playerWrapDisplay == true) {
        this.playerWrapDisplay = false;
      } else {
        this.playerWrapDisplay = true;
      }
    },
    getMediaSource() {
      try {
        this.mediaList = require("@/assets/data/mediaList.json");
        this.audioTitle = this.mediaList[this.audioId].title;
        this.artist = this.mediaList[this.audioId].artist;
        this.cover = this.mediaList[this.audioId].cover;
        this.videoOptions.sources[0].src = this.mediaList[this.audioId].url;
        this.videoOptions.sources[0].type = this.mediaList[this.audioId].type;
        this.initAudio();
        this.timer = new Date().getTime();
      } catch (error) {
        console.error(error);
        this.audioTitle = "获取音频列表失败";
      }
      this.timer = new Date().getTime();
    },
    getImage(img) {
      try {
        const a = require("@/assets/img/radio/" + img);
        return a;
      } catch (err) {
        console.log(err);
        return require("@/assets/img/function/pic.svg");
      }
    },
    audioControl() {
      if (this.audioStatus == "playing") {
        this.$emit("audio_pause");
        this.audioObj.pause();
        this.audioStatus = "";
      } else {
        this.$emit("audio_play");
        this.audioObj.play();
        this.audioStatus = "playing";
      }
    },
    audioChange(changeType) {
      if (this.audioId >= 0 && this.audioId <= this.mediaList.length) {
        //上一首
        if (changeType == "pre" && this.audioId > 0) {
          this.audioId = this.audioId - 1;
        } else if (
          changeType == "next" &&
          this.audioId < this.mediaList.length - 1
        ) {
          this.audioId = this.audioId + 1;
        } else if (
          changeType == "next" &&
          this.audioId == this.mediaList.length - 1
        ) {
          this.audioId = 0;
        } else {
          return;
        }
        this.audioTitle = this.mediaList[this.audioId].title;
        this.artist = this.mediaList[this.audioId].artist;
        this.videoOptions.sources[0].src = this.mediaList[this.audioId].url;
        this.videoOptions.sources[0].type = this.mediaList[this.audioId].type;
        this.cover = this.mediaList[this.audioId].cover;
        this.timer = new Date().getTime();
        setTimeout(() => {
          this.$emit("audio_play");
          this.audioObj.play();
          this.audioStatus = "playing";
        }, 500);
      }
    },
    audioSwitch(index) {
      this.audioId = index;
      this.audioTitle = this.mediaList[index].title;
      this.artist = this.mediaList[index].artist;
      this.videoOptions.sources[0].src = this.mediaList[index].url;
      this.videoOptions.sources[0].type = this.mediaList[index].type;
      this.cover = this.mediaList[index].cover;
      this.timer = new Date().getTime();
      setTimeout(() => {
        this.$emit("audio_play");
        this.audioObj.play();
        this.audioStatus = "playing";
      }, 500);
    },
    initAudio() {
      //初始化视频方法
      this.timer = new Date().getTime();
    },
    updatePlayerItem(playerItem) {
      this.audioObj = playerItem;
    },
  },
  watch: {
    audioId(newId) {
      localStorage.audio_index = newId;
    },
    // playerWrapDisplay(newStat) {
    //   if (newStat) {
    //     setTimeout(()=>{
    //       console.log(this.$refs.content.offsetHeight);
    //       if (this.$refs.content.offsetHeight != 0){
    //         this.listHeight = `${this.$refs.content.offsetHeight}px`;
    //         this.$refs.content.style.maxHeight = this.listHeight;
    //       }
    //     },500);

    //     // this.$refs.content.style.display="flex";
    //     // setTimeout(()=>{
    //     //   var height = this.listHeight;
    //     //   this.$refs.content.style.height = height;
    //     // },50);
    //     // this.$refs.content.style.opacity = 1;
    //   } else {
    //     // this.$refs.content.style.height = 0;
    //     // this.$refs.content.style.opacity = 0;
    //     // setTimeout(()=>{
    //     //   this.$refs.content.style.display="none";
    //     // },200);
    //   }
    // },
  },
  beforeCreate() {},
  mounted() {
    if (localStorage.audio_index)
      this.audioId = Number(localStorage.audio_index);
    this.getMediaSource();
    // this.$nextTick(el => {
    //   console.log(this.listHeight);
    //   this.listHeight = `${this.$refs.content.offsetHeight}px`;
    //   console.log(this.listHeight);
    //   this.$refs.content.style.height = 0;
    //   this.$refs.content.style.display="none";
    // });
  },
  beforeDestroy() {},
};
</script>

<style>
@keyframes bounce-in {
  0% {
    opacity: 0;
    /* transform: scale(0.8); */
    /* max-height: 0; */
    /* filter: blur(10px); */
  }
  30% {
    /* transform: scale(1.01); */
    opacity: 1;
  }
  100% {
    /* transform: scale(1); */
    opacity: 1;
  }
}
@keyframes bounce-out {
  0% {
    /* transform: scale(1); */
    opacity: 1;
  }
  30% {
    /* transform: scale(1.01); */
    opacity: 0.8;
  }
  100% {
    /* transform: scale(0.8); */
    /* max-height: 0; */
    opacity: 0;
    /* filter: blur(1px); */
  }
}
/* .fade-in-out-enter-active {
  transform-origin: left top;
  animation: bounce-in 0.2s ease-out;
}
.fade-in-out-leave-active {
  transform-origin: left top;
  animation: bounce-out 0.2s ease-in;
} */
:root {
  --audio-player-width: 400px;
  --audio-player-height: 90px;
  --audio-content-height: calc(100vh - 250px);
}
#player {
  /* display: flex; */
  height: 100%;
  width: 100%;
}
.player-entry-cover-image {
  position: relative;
  width: 100%;
  z-index: 1000;
}
.player-wrap {
  width: 100%;
  /* display: flex; */
  /* flex-direction: column;
  align-items: space-between;
  justify-content: space-between; */
  /* border-radius: 0 10px 10px 0; */
  transition: all 0.2s ease;
  background: linear-gradient(-45deg, rgb(83, 83, 83), rgb(145, 145, 145));
  height: 100%;
}
.player-entry {
  height: 100%;
  box-sizing: border-box;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
  padding: 20px;
}
.player-cover {
  width: var(--audio-content-height);
  /* height: max-content; */
  /* min-height: var(--audio-content-width); */
  height: var(--audio-content-height);
  max-width: calc(100vw - 60px);
  max-height: calc(100vw - 60px);
  /* max-width: var(--); */
  background: var(--first-assist-color);
  border-radius: 10px;
  overflow: hidden;
  transition: all 0.2s ease;
  cursor: pointer;
  /* flex:1; */
  flex-shrink: 0;
}
.player-cover:hover {
  /* transform: scale(1.05);
  transition: all 0.2s cubic-bezier(0.34, 0.07, 0, 0.87); */
}
.player-cover:active {
  /* transform: scale(0.95);
  transition: all 0.2s cubic-bezier(0.34, 0.07, 0, 0.87); */
  filter:brightness(70%);
}
.player-cover-image {
  position: relative;
  width: 100%;
  /* z-index: 1000; */
}

.player-expanded-content {
  /* z-index:10000; */

  /* width: 50vw; */
  /* margin:10px; */
  /* border-radius: 10px; */
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  /* border-top: 5px solid #92929213; */
  transition: all 0.2s ease;
  overflow: scroll;
  width: 100%;
  box-sizing: border-box;
  background: var(--elem-color);
  padding: 20px;
  height: 100%;
}
.player-info {
  color: var(--main-color);
  color: #fff;
  display: flex;
  flex-direction: column;
  margin: 10px 0;
  flex: 1;
  cursor: pointer;
  border-radius: 5px;
  padding: 3px 5px;
}
.player-info:active {
  /* transform: scale(0.95);
  transition: all 0.2s cubic-bezier(0.34, 0.07, 0, 0.87); */
  background: #00000023;
}
.player-control-area {
  width: var(--audio-content-width);
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: flex-start;
}
.player-control-btn-large,
.player-control-btn-medium,
.player-control-btn-small {
  color: var(--main-color);
  color: #fff;
  border-radius: 50%;
  flex-shrink: none;
  /* background: #000; */
  transition: all 0.2s ease;
  cursor: pointer;
}

.player-control-btn-large {
  font-size: 55px !important;
}
.player-control-btn-medium {
  font-size: 55px !important;
  width: 60px;
  height: 60px;
}
.player-control-btn-playpause {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 60px;
  height: 60px;
  background: var(--main-color-opa);
  border-radius: 50%;
  transition: all 0.2s ease;
  margin: 0 10px 0 0;
}

.player-control-btn-playpause:hover,
.player-control-btn-medium:hover,
.player-control-btn-small:hover {
  background: #ffffff78;
  transform: scale(1.1);
  box-shadow: 0 5px 10px 2px #00000023;
  transition: all 0.2s ease;
}
.player-control-btn-playpause:active,
.player-control-btn-medium:active,
.player-control-btn-small:active {
  transform: scale(0.96);
  box-shadow: 0 3px 3px 1px #00000023;
}
.player-media {
  display: none !important;
  width: 0;
  height: 0;
  overflow: hidden;
  position: relative;
  top: 0;
  width: 100%;
  height: var(--audio-content-height);
}
.player-info-title {
  font-size: 24px;
  /* font-family: 'Google Sans'; */
  font-weight: 800;
}
.player-info-artist {
  font-size: 19px;
}
.media-list {
  /* max-height: 0; */
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: space-between;
  justify-content: center;
  height: 100%;
}
.media-item {
  /* width: 100%; */
  padding: 10px 12px;
  box-sizing: border-box;
  border-bottom: 1px solid var(--main-color-opa);
  display: flex;
  justify-content: flex-start;
  align-items: center;
  cursor: pointer;
  /* border-radius: 5px; */
}
.media-item:last-child {
  border-bottom: none;
}
/* .media-item:hover {
  background: #00000023;
  transition: all 0.2s ease;
} */
.media-item:active {
  background: var(--first-assist-color);
  /* transition: all 0.2s ease; */
}
.media-item-image {
  width: 40px;
  height: 40px;
  background: var(--first-assist-color);
  border-radius: 3px;
  overflow: hidden;
}
.media-item-image-s {
  width: 100%;
}
.media-item-content {
  margin: 0 0 0 10px;
  display: flex;
  flex-direction: column;
  color: var(--main-color);
}
.card-details{
  color: var(--inactive-color);

}
</style>