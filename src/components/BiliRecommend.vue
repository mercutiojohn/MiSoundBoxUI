<template>
  <div class="bili">
    <!-- <div class="bili-item" v-for="(item,index) in data" :key="index">
      <a :href="item.url"><span class="bili-content">{{ item.title }}</span></a>
      </div> -->
    <div class="bili-item">
      <a :href="'https://www.bilibili.com/video/av' + data[0].args.aid">
        <div class="item-cover">
          <img :src="getCover(data[0].cover)" alt="" srcset="" />
        </div>
        <div class="item-content">
          <span class="item-title">
            {{ data[0].title }}
          </span>
          <div class="item-desc">
            <div class="desc-first-line">
              <span class="desc-name">{{ data[0].args.up_name }}</span>
              <span class="desc-cate"
                >{{ data[0].desc_button.text }}<br
              /></span>
            </div>
            <div class="desc-second-line">
              <span class="desc-plays"
                >播放{{ data[0].cover_left_text_1 }}</span
              >
              <span class="desc-danmakus"
                >弹幕{{ data[0].cover_left_text_2 }}</span
              >
            </div>
          </div>
        </div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "BiliRecommend",
  data() {
    return {
      timer: "",
      cover_0: "",
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
            "",
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
          uri:
            "",
        },
      ],
    };
  },

  methods: {
    getBili() {
      console.log("hello");
      this.$axios
        .get(this.$store.state.apiPath + "/bilibili/main")
        .then(({ data }) => {
          this.data = data.content.data.items;
        })
        .catch(console.error);
    },
    getCover(url) {
      let coverUrl = encodeURIComponent(url);
      this.$axios
        .get(this.$store.state.apiPath + "/bilibili/get-cover?url=" + coverUrl)
        .then(({ data }) => {
          this.cover_0 = "data:image/jpg;base64," + data;
          return "data:image/jpg;base64," + data;
        })
        .catch(console.error);
      return this.cover_0;
    },
  },
  created() {
    this.timer = setInterval(this.getBili(), 1000);
  },
  mounted() {
    // this.getBili();
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
.bili-item {
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
  bottom: 0;
  background-image: linear-gradient(#00000000,#000000ee);
  padding: 30px;
  border-radius: 0 0 30px 30px;
}
.item-title {
  font-size: 27px;
  height: 100%;
  padding: 0 0 10px 0;
  color: #ffffff;
  font-weight: 800;
  max-height: 50px;
  text-overflow: ellipsis;
}
.item-desc {
  font-size: 20px;
  color: #ffffff78;
  z-index: 100;
}
</style>