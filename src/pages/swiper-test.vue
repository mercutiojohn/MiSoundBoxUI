<!-- 组件的模板 -->
<template>
  <div class="car">
    <!-- 第二个swiper -->
    <div class="swiper2">
      <swiper :options="swiperOption2">
        <swiper-slide>
          <div>第一个</div>
        </swiper-slide>
        <swiper-slide>
          <div>第二个</div>
        </swiper-slide>
        <swiper-slide>
          <div>第三个</div>
        </swiper-slide>
        <swiper-slide>
          <div>第四个</div>
        </swiper-slide>
        <swiper-slide>
          <div>第五个</div>
        </swiper-slide>
      </swiper>
    </div>
  </div>
</template>

<!-- 组件的模型 -->
<script>
export default {
  // 组件的名称
  name: "swiper-test",
  data() {
    return {
      swiperOption2: {
        autoplay: {
          delay: 3000,
          disableOnInteraction: false,
        },
        loop: true,
        direction: "vertical",
        slidesPerView: "auto",
      },
    };
  },
  methods: {
    onSwiper(swiper) {
      console.log(swiper);
    },
    onSlideChange() {
      console.log("slide change");
    },
  }
};
</script>

<!-- 组件的样式 -->
<style scoped>
.swiper2 {
  width: 300px;
  height: 600px;
  margin: 20px auto;
}
.swiper2 .swiper-container {
  height: 100%;
}
.swiper2 .swiper-slide {
  height: 200px;
}
.swiper2 .swiper-slide div {
  box-sizing: border-box;
  height: 100%;
  line-height: 200px;
  background-color: lightblue;
  text-align: center;
  border-bottom: 1px solid #e5e5e5;
}
</style>