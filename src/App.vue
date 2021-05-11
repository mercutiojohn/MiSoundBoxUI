<template>
  <div id="all">
    <!-- <swiper :options="swiperOption"> -->
      <!-- <swiper-slide> -->
        <div class="main-page">
          <HeaderBar />
          <!-- <div id="fake"></div>  -->
          <!-- <transition name="fade" mode="out-in"> -->
            <!-- <keep-alive> -->
            <router-view></router-view>
            <!-- </keep-alive> -->
          <!-- </transition> -->
          <!-- <Course /> -->
        </div>
      <!-- </swiper-slide> -->
      <!-- <swiper-slide> -->
        <EmbedFrame
          url="https://www.dida365.com/webapp/#q/all/today"
          title="滴答清单"
          height="calc(100vh - 1px)"
          :hideExpand="true"
          :hideActionBar="true"
        />
      <!-- </swiper-slide> -->
      <!-- <swiper-slide class=""></swiper-slide> -->
    <!-- </swiper> -->
  </div>
</template>
<script>
import HeaderBar from "@/components/HeaderBar";
import EmbedFrame from "@/components/EmbedFrame";

export default {
  name: "App",
  components: {
    HeaderBar,
    EmbedFrame,
  },
  mounted() {
    const script = document.createElement("script");
    script.src =
      "https://v1.cnzz.com/z_stat.php?id=1279258067&web_id=1279258067";
    script.language = "JavaScript";
    document.body.appendChild(script);
  },
  data() {
    return {
      swiperOption: {
        direction: "vertical",
        slidesPerView: 1,
        // autoHeight: true
        spaceBetween: 0,
        loop: true,
      },
    };
  },
  methods: {},
  watch: {
    $route: {
      handler(to, from) {
        setTimeout(() => {
          //避免首次获取不到window._czc
          if (window._czc) {
            let location = window.location;
            let contentUrl = location.pathname + location.hash;
            let refererUrl = "/";
            // 用于发送某个URL的PV统计请求，
            window._czc.push(["_trackPageview", contentUrl, refererUrl]);
            window._czc.push(["_setAutoPageview", false]);
          }
        }, 300);
      },
      immediate: true, // 首次进入页面即执行
    },
  },
};
</script>

<style>
@import "./main.css";

 *{ -webkit-tap-highlight-color: rgba(0, 0, 0, 0);}

body {
  /* perspective:550px; */
  margin: 0;
  padding: 0;
  /* background: url(https://bing.rthe.net/wallpaper) no-repeat fixed center/cover; */
}
input,
select,
textarea {
  outline: none;
  filter: chroma(color=#000000);
}

#all {
  margin: 0;
  padding: 0;
  width: 100vw;
  min-height: 100vh;
  font-family: "SF Pro Display", "苹方", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* background: url(https://bing.rthe.net/wallpaper) no-repeat fixed center/cover; */
  /* text-align: center; */
  /* color: #2c3e50; */
  /* margin-top: 60px; */
}
/* #fake {
  height: 1000px;
  width: 100px;
} */
.main-page {
  height: 100vh;
  border-radius: 25px;
  overflow: hidden;
}
</style>
