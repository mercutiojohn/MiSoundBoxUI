<template>
  <div id="mobile">
    <div class="mobile-tools-list">
      <swiper :options="swiperOption2">
        <swiper-slide class="first-width fake-margin oline">
          <div class="mobile-tool-item first-width-inner disable-scroll mobile-tool-item-gel">
            <DynamicMainContent />
          </div>
        </swiper-slide>
        <swiper-slide class="force-width fake-margin oline">
          <div class="mobile-tool-item mobile-tool-item-gel">
            <Weather />
          </div>
        </swiper-slide>
        <swiper-slide class="force-width fake-margin oline">
          <div class="mobile-tool-item mobile-tool-item-gel">
            <Countdown />
          </div>
        </swiper-slide>

        <swiper-slide class="force-width fake-margin oline">
          <div class="mobile-tool-item mobile-tool-item-gel">
            <Player />
          </div>
        </swiper-slide>
        <swiper-slide class="first-width fake-margin oline">
          <div class="mobile-tool-item first-width-inner">
            <BiliRecommend />
          </div>
        </swiper-slide>
        <swiper-slide class="force-width fake-margin oline">
          <div class="mobile-tool-item mobile-tool-item-gel">
            <Weibo />
          </div>
        </swiper-slide>
      </swiper>
      <!-- <div class="mobile-tool-item first-width-inner">
        <DynamicMainContent />
      </div>
      <div class="mobile-tool-item">
        <Countdown />
      </div>
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
import Weibo from "@/components/Weibo";
import BiliRecommend from "@/components/BiliRecommend";
import Weather from "@/components/Weather";
export default {
  name: "MobileHome",
  components: {
    Player,
    Countdown,
    Homeworks,
    DynamicMainContent,
    EmbedFrame,
    Weibo,
    BiliRecommend,
    Weather,
  },
  data() {
    return {
      swiperOption2: {
        // autoplay: {
        //   delay: 3000,
        //   disableOnInteraction: false,
        // },
        loop: false,
        direction: "horizontal",
        spaceBetween: 0,
        slidesPerView: "auto",
        observer: true, //修改swiper自己或子元素时，自动初始化swiper
        observeParents: true, //修改swiper的父元素时，自动初始化swiper
        keyboard: {
          enabled: true,
        },
      },
    };
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
:root {
  --medium-width: calc(100vh - 64px);
  --large-width: calc(100vw - 100px);
  --radius:30px;
}
body {
  /* background: #000; */
}
.first-width-inner {
  width: calc(100vw - 100px);
}
.first-width-inner * {
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
  /* margin: 0 0 0 20px; */
  background: var(--elem-color);
  border-radius: var(--radius);
  /* box-shadow: 0 2px 6px 1px #00000014; */
  /* background: #ffffff75;
  backdrop-filter: blur(30px) saturate(180%);
  border-radius: 10px;
  box-shadow: 0 5px 8px 3px #00000014; */
  overflow: hidden;
  overflow-y: scroll;
  flex-shrink: 0;
  /* min-width: calc(100vh - 64px); */
  max-width: calc(100vw - 20px);
  transition: all 0.1s ease;
  /* transition-delay: 50ms; */
}
.mobile-tool-item:first-child {
  /* overflow: hidden; */
  /* margin: 0 0 0 20px; */
}

/* .mobile-tool-item:last-child{
  outline:10px
} */
.mobile-tool-item-gel:active {
  transform: scale(0.98);
  transition-delay: 0;
}
.force-width {
  width: var(--medium-width);
}
.first-width {
  width: var(--large-width);
}
.fake-margin-first {
  padding: 0 5px 0 10px;
}
.fake-margin {
  padding: 0 10px;
}
.oline {
  /* border: 1px solid #fff; */
}
.disable-scroll {
  overflow: hidden;
}
.swiper-slide {
  transition: all 0.2s ease;
}

.swiper-slide-prev {
  transform: scale(0.86);
}
</style>