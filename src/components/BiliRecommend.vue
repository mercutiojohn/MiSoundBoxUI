<template>
  <div class="bili">
    <!-- <div class="bili-item" v-for="(item,index) in data" :key="index">
      <a :href="item.url"><span class="bili-content">{{ item.title }}</span></a>
      </div> -->
    <swiper :options="biliSwiperOptions">
      <swiper-slide
        v-for="(item, index) in data"
        :key="index"
        class="bili-slide"
      >
        <div class="bili-item">
          <a :href="getVidUrl(item)" :target="!toApp ? '_blank' : ''">
            <div class="item-cover">
              <img :src="getCover(item.cover,index)" alt="" srcset="" />
              <!-- <img :src="covers[0]" alt="" srcset="" /> -->
              <!-- <img :src="getCover(item.cover,index)" alt="" srcset="" /> -->
              <!-- <img :src="getCover(item.cover,index)" alt="" srcset="" /> -->
            </div>
            <div class="item-content">
              <span class="item-title">
                {{ item.title }}
              </span>
              <div class="item-desc">
                <div class="desc-first-line">
                  <span class="desc-name">{{ item.args.up_name }}</span>
                  <span class="desc-cate"
                    >{{ item.desc_button.text }}<br
                  /></span>
                </div>
                <div class="desc-second-line">
                  <span class="desc-plays">
                    播放 {{ item.cover_left_text_1 }}
                  </span>
                  <span class="desc-danmakus">
                    弹幕 {{ item.cover_left_text_2 }}
                  </span>
                  <span class="desc-length">
                    时长 {{ item.cover_right_text }}
                  </span>
                </div>
              </div>
            </div>
          </a>
        </div>
      </swiper-slide>
    </swiper>
  </div>
</template>

<script>
export default {
  name: "BiliRecommend",
  data() {
    return {
      timer: "",
      cover_0: "",
      toApp: true,
      covers: [],
      index: 0,
      timer_fetch: "",
      biliSwiperOptions: {
        // autoplay: {
        //   delay: 3000,
        //   disableOnInteraction: false,
        // },
        loop: false,
        direction: "vertical",
        spaceBetween: 0,
        slidesPerView: "auto",
        longSwipesRatio: 0.2,
        observer: true, //修改swiper自己或子元素时，自动初始化swiper
        observeParents: true, //修改swiper的父元素时，自动初始化swiper
        keyboard: {
          enabled: true,
        },
        autoHeight: true,
      },
      data: [
        {
          args: {
            aid: 803056651,
            rid: 219,
            rname: "汪星人",
            tid: 9955,
            tname: "汪星人",
            up_id: 1463519331,
            up_name: "正在加载",
          },
          can_play: 1,
          card_goto: "av",
          card_type: "small_cover_v2",
          cover:
            "http://i0.hdslb.com/bfs/archive/3878ec2eb2813527ea929fbdc213fc9f163ec908.jpg",
          cover_left_icon_1: 1,
          cover_left_icon_2: 3,
          cover_left_text_1: "12.1万",
          cover_left_text_2: "1195",
          cover_right_text: "5:01",
          desc_button: {
            event: "channel_click",
            event_v2: "channel",
            text: "正在加载",
            type: 1,
            uri: "bilibili://pegasus/channel/9955",
          },
          goto: "av",
          idx: 1620734408,
          official_icon: 16,
          param: "",
          player_args: {
            aid: 803056651,
            cid: 336604845,
            duration: 301,
            type: "av",
          },
          three_point: {
            dislike_reasons: [
              {
                id: 4,
                name: "UP主:隔壁老李的矿山日记",
              },
              {
                id: 2,
                name: "分区:汪星人",
              },
              {
                id: 3,
                name: "频道:汪星人",
              },
              {
                id: 1,
                name: "不感兴趣",
              },
            ],
            feedbacks: [
              {
                id: 1,
                name: "恐怖血腥",
              },
              {
                id: 2,
                name: "色情低俗",
              },
              {
                id: 3,
                name: "封面恶心",
              },
              {
                id: 4,
                name: "标题党/封面党",
              },
            ],
            watch_later: 1,
          },
          three_point_v2: [
            {
              title: "添加至稍后再看",
              type: "watch_later",
            },
            {
              reasons: [
                {
                  id: 1,
                  name: "恐怖血腥",
                },
                {
                  id: 2,
                  name: "色情低俗",
                },
                {
                  id: 3,
                  name: "封面恶心",
                },
                {
                  id: 4,
                  name: "标题党/封面党",
                },
              ],
              subtitle: "(选择后将优化首页此类内容)",
              title: "反馈",
              type: "feedback",
            },
            {
              reasons: [
                {
                  id: 4,
                  name: "UP主:隔壁老李的矿山日记",
                },
                {
                  id: 2,
                  name: "分区:汪星人",
                },
                {
                  id: 3,
                  name: "频道:汪星人",
                },
                {
                  id: 1,
                  name: "不感兴趣",
                },
              ],
              subtitle: "(选择后将减少相似内容推荐)",
              title: "不感兴趣",
              type: "dislike",
            },
          ],
          title: "正在加载",
          uri: "",
        },
        {
          args: {
            aid: 803056651,
            rid: 219,
            rname: "汪星人",
            tid: 9955,
            tname: "汪星人",
            up_id: 1463519331,
            up_name: "正在加载",
          },
          can_play: 1,
          card_goto: "av",
          card_type: "small_cover_v2",
          cover:
            "http://i0.hdslb.com/bfs/archive/3878ec2eb2813527ea929fbdc213fc9f163ec908.jpg",
          cover_left_icon_1: 1,
          cover_left_icon_2: 3,
          cover_left_text_1: "0",
          cover_left_text_2: "0",
          cover_right_text: "5:01",
          desc_button: {
            event: "channel_click",
            event_v2: "channel",
            text: "正在加载",
            type: 1,
            uri: "bilibili://pegasus/channel/9955",
          },
          goto: "av",
          idx: 1620734408,
          official_icon: 16,
          param: "",
          player_args: {
            aid: 803056651,
            cid: 336604845,
            duration: 301,
            type: "av",
          },
          three_point: {
            dislike_reasons: [
              {
                id: 4,
                name: "UP主:隔壁老李的矿山日记",
              },
              {
                id: 2,
                name: "分区:汪星人",
              },
              {
                id: 3,
                name: "频道:汪星人",
              },
              {
                id: 1,
                name: "不感兴趣",
              },
            ],
            feedbacks: [
              {
                id: 1,
                name: "恐怖血腥",
              },
              {
                id: 2,
                name: "色情低俗",
              },
              {
                id: 3,
                name: "封面恶心",
              },
              {
                id: 4,
                name: "标题党/封面党",
              },
            ],
            watch_later: 1,
          },
          three_point_v2: [
            {
              title: "添加至稍后再看",
              type: "watch_later",
            },
            {
              reasons: [
                {
                  id: 1,
                  name: "恐怖血腥",
                },
                {
                  id: 2,
                  name: "色情低俗",
                },
                {
                  id: 3,
                  name: "封面恶心",
                },
                {
                  id: 4,
                  name: "标题党/封面党",
                },
              ],
              subtitle: "(选择后将优化首页此类内容)",
              title: "反馈",
              type: "feedback",
            },
            {
              reasons: [
                {
                  id: 4,
                  name: "UP主:隔壁老李的矿山日记",
                },
                {
                  id: 2,
                  name: "分区:汪星人",
                },
                {
                  id: 3,
                  name: "频道:汪星人",
                },
                {
                  id: 1,
                  name: "不感兴趣",
                },
              ],
              subtitle: "(选择后将减少相似内容推荐)",
              title: "不感兴趣",
              type: "dislike",
            },
          ],
          title: "正在加载",
          uri: "",
        },
      ],
    };
  },

  methods: {
    getBili() {
      this.$axios
        .get(this.$store.state.apiPath + "/bilibili/main")
        .then(({ data }) => {
          this.data = data.content.data.items;
        })
        .catch(console.error);
    },
    async getCover(url,index) {
      let coverUrl = encodeURIComponent(url);
      let base;
      await this.$axios
        .get(this.$store.state.apiPath + "/bilibili/get-cover?url=" + coverUrl)
        .then(({ data }) => {
          console.log("got:" + index +" "+coverUrl);
          base = "data:image/jpg;base64,"+data;
          return base;
        })
        .catch(console.error);
    },
    getAllCover() {
      let _this = this;
      for (var i = 0; i < this.data.length; i++) {
        // this.covers[i] = this.getCover(_this.data[i].cover,i);
        this.getCover(_this.data[i].cover,i);
      }
    },

    getVidUrl(data) {
      if (this.toApp) return data.uri;
      else return "https://www.bilibili.com/video/av" + data.args.aid;
    },
  },
  created() {
    
  },
  mounted() {
    if (localStorage.bili_data) {
      this.data = JSON.parse(localStorage.bili_data);
    }
    this.timer_fetch = setTimeout(this.getBili(), 600000);
  },
  watch: {
    data(newVal) {
      localStorage.bili_data = JSON.stringify(newVal);
    },
    // covers(newVal) {
    //   var i = 0;
    //   newVal.forEach((element) => {
    //     window.localStorage.setItem("bili_cover_" + i, element);
    //     i++;
    //   });
    // },
  },
  beforeDestroy() {
    localStorage.bili_data = JSON.stringify(this.data);
    var i = 0;
    this.covers.forEach((element) => {
      window.localStorage.setItem("bili_cover_" + i, element);
      i++;
    });
  },
};
</script>

<style>
:root {
  --content-max-width: calc(var(--large-width));
  --content-max-height: calc(var(--medium-width));
}
.bili {
  /* background-color: linear-gradient(#b66969,#0080ff) ; */
  /* background-clip: border-box;
    background-attachment: fixed;
    background-origin: border-box;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover; */
  /* background-color: #000; */
  /* background-image: url(https://source.unsplash.com/random/800x600); */
  flex: 1;
  display: flex;
  align-items: flex-start;
  justify-content: flex-start;
  padding: 0;
  /* height: 100%; */
  flex-direction: column;
}
.bili-slide {
  transition: all 0.2s ease;
}
.bili-slide:active {
  transform: scale(0.98);
}
.bili-item {
  border-radius: var(--radius);
  overflow: hidden;
  margin: 0 0 10px 0;
  /* border-radius: var(--radius); */
  /* overflow: hidden; */
}
.bili-content {
  /* text-shadow: 0 0 20px #00000094; */
  width: 100%;
  color: var(--main-color);
  display: flex;
  justify-content: flex-start;
  font-size: 24px;
  font-weight: 600;
}
.bili-item a {
  text-decoration: none;
  color: var(--elem-color);
}
.item-cover {
  background: #000;
  width: var(--content-max-width);
  height: var(--content-max-height);
  /* overflow: hidden; */
  display: flex;
  align-items: center;
  z-index: 0;
  /* justify-content: center; */
}
.item-cover > img {
  width: 100%;
}
.item-content {
  width: var(--content-max-width);
  box-sizing: border-box;
  position: absolute;
  bottom: 10px;
  background-image: linear-gradient(#00000000, #000000ee);
  /* background-color: #00000090; */
  padding: 70px 40px 20px 30px;
  border-radius: 0 0 30px 30px;
}
.item-title {
  display: flex;
  font-size: 23px;
  height: 100%;
  margin: 0 0 10px 0;
  color: #ffffff;
  font-weight: 800;
  max-height: 64px;
  text-overflow: ellipsis;
  /* white-space: pre-wrap; */
  overflow: hidden;
  /*
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2; */
}
.item-desc {
  font-size: 15px;
  color: #bebebede;
  /* z-index: 100; */
  display: flex;
  justify-content: space-between;
}
</style>