<template>
  <div class="sub-page">
    <swiper :options="swiperOption">
      <swiper-slide class="sub-slide">
        <div class="slider slider-left"></div>
        <EmbedFrame
          url="https://www.dida365.com/webapp/#q/all/today"
          title="滴答清单"
          height="calc(100vh - 1px)"
          :hideExpand="true"
          :hideActionBar="true"
        />
        <div class="slider slider-right"></div>
      </swiper-slide>
      <swiper-slide class="sub-slide">
        <div class="slider slider-left"></div>
        <BigClock />
        <div class="slider slider-right"></div>
      </swiper-slide>
      <swiper-slide class="sub-slide">
        <DynamicMainContent />
      </swiper-slide>
      
    </swiper>
  </div>
</template>

<script>
import EmbedFrame from "@/components/EmbedFrame";
import DynamicMainContent from "@/components/DynamicMainContent";
import BigClock from "@/components/BigClock";

export default {
  name: "SubPage",
  components: {
    EmbedFrame,
    DynamicMainContent,
    BigClock
  },
  data() {
    return {
      swiperOption: {
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
  computed: {},
  watch: {},
  methods: {},
  created() {},
  mounted() {},
  beforeDestroy() {},
};
</script>

<style>
.sub-page {
  height: 100vh;
  width: 100%;
  overflow: hidden;
}
.sub-slide {
  /* width:100%; */
  /* border-radius: 30px; */
  height: 100vh;
  overflow: hidden;
}
.slider {
  width: 30px;
  background: transparent;
  top: calc(50vh - 50px);
  height: 100px;
  position: absolute;
  top: 0;
  height: 100vh;
}
.slider-left {
  /* border-radius: 0 20px 20px 0; */
  left: 0;
}
.slider-right {
  /* border-radius: 20px 0 0 20px; */
  right: 0;
}
</style>