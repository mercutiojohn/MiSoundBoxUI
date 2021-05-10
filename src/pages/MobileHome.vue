<template>
  <div id="mobile">
    
    <div class="mobile-tools-list">
<<<<<<< HEAD
      <swiper :options="swiperOption2">
      <swiper-slide class="mobile-tool-item main-width">
        <!-- <div class="mobile-tool-item main-width"> -->
          <DynamicMainContent />
        <!-- </div> -->
      </swiper-slide>
      <swiper-slide class="mobile-tool-item force-width">
          <Countdown />
      </swiper-slide>
      <swiper-slide class="mobile-tool-item force-width">
          <Player />
      </swiper-slide>
      <swiper-slide class="mobile-tool-item force-width">
      </swiper-slide>
    </swiper>
      <!-- <div class="mobile-tool-item main-width">
=======
      <div class="mobile-tool-item main-width">
>>>>>>> mobile
        <DynamicMainContent />
      </div>
      <div class="mobile-tool-item">
        <Countdown />
      </div>
<<<<<<< HEAD
=======

>>>>>>> mobile
      <div class="mobile-tool-item">
        <Player />
      </div>
      <div class="mobile-tool-item"></div> -->
      <!-- <div class="mobile-tool-item">
        <Homeworks />
      </div> -->
    </div>
  </div>
</template>

<script>
import Player from "@/components/Player";
import Countdown from "@/components/Countdown";
import Homeworks from "@/components/Homeworks";
import DynamicMainContent from "@/components/DynamicMainContent";
import EmbedFrame from "@/components/EmbedFrame";
export default {
  name: "MobileHome",
  components: {
    Player,
    Countdown,
    Homeworks,
    DynamicMainContent,
<<<<<<< HEAD
    EmbedFrame,
  },
  data() {
    return {
      swiperOption2: {
        autoplay: {
          delay: 3000,
          disableOnInteraction: false,
        },
        loop: false,
        direction: "horizontal",
        spaceBetween: 10,
        slidesPerView: "auto",
      },
    };
=======
>>>>>>> mobile
  },
  methods: {
    linktab() {
      let goUrl = this.isMobile();
      if (goUrl === 1) {
        //移动端地址
        // location = "http://127.0.0.1:8043";
        // alert("hello");
      } else {
        //   location = "/";
        //PC地址
      }
    },
    isMobile() {
      let flag = navigator.userAgent.match(
        /(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i
      );
      let goUrl = flag ? 1 : 0;
      return goUrl;
    },
  },
  mounted() {
    this.linktab();
  },
  created() {},
};
</script>

<style>
body {
  /* background: #000; */
}
.main-width {
  width: calc(100vw - 100px);
}
.main-width * {
  zoom: 1;
}
.mobile-tool-item > * {
  zoom: 1;
}
.mobile-tools-list {
  width: 100vw;
  overflow: scroll;
  display: flex;
  box-sizing: border-box;
  padding: 0 0 10px 0;
  /* margin: 0 20px; */
  height: calc(100vh - 64px);
}
.mobile-tool-item {
  height: 100%;
  margin: 0 0 0 10px;
  background: var(--elem-color);
  border-radius: 30px;
  /* box-shadow: 0 2px 6px 1px #00000014; */
  /* background: #ffffff75;
  backdrop-filter: blur(30px) saturate(180%);
  border-radius: 10px;
  box-shadow: 0 5px 8px 3px #00000014; */
  overflow: hidden;
  overflow-y: scroll;
  flex-shrink: 0;
  min-width: calc(100vh - 64px);

}
.mobile-tool-item:first-child{
  margin: 0 0 0 20px;
  
}
/* .mobile-tool-item:last-child{
  outline:10px
} */
.force-width{
  width: calc(100vh - 64px);
}
</style>