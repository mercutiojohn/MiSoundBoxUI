<template>
  <div class="weibo">
      <div class="weibo-title">
        <span>微博热搜</span>
      </div>
      <div class="weibo-item" v-for="(item,index) in data" :key="index">
      <a :href="item.url"><span class="weibo-content">{{ item.title }}</span></a>
      </div>
    </div>

</template>

<script>

export default {
  name: "weibo",
  data() {
    return {
      data: [{
            "title": "正在加载",
            "url": ""
        }]
    };
  },

  methods: {
    getWeibo() {
      this.$axios
        .get(this.$store.state.apiPath + "/weibo")
        .then(({ data }) => {
          this.data = data.content.data;
        })
        .catch(console.error);
    },
  },
  created() {},
  mounted() {
    this.getWeibo();
  },
};
</script>

<style>
.weibo{
  /* background-color: linear-gradient(#b66969,#0080ff) ; */
    /* background-clip: border-box;
    background-attachment: fixed;
    background-origin: border-box;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover; */
    /* background-color: #000; */
    /* background-image: url(https://source.unsplash.com/random/800x600); */
    flex:1;
    display: flex;
    align-items: flex-start;
    justify-content: flex-start;
    padding: 20px;
    /* height: 100%; */
    flex-direction: column;
}
.weibo-item{
  margin:10px 0;
}
.weibo-content {
  /* text-shadow: 0 0 20px #00000094; */
  width: 100%;
  color: var(--main-color);
  display: flex;
  justify-content: flex-start;
  font-size: 20px;
  font-weight: 600;
}
.weibo-item a{
  text-decoration: none;
}
.weibo-title{
  border-bottom: 1px solid var(--first-assist-color);
  width: 100%;
  padding: 0 0 10px 0;
  color:var(--main-color);
}
</style>