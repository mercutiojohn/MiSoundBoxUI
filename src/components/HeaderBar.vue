<template>
  <div class="header-bar-container">
    <!-- <ul class="header-bar-tablist"  @click="printPath()">
      <router-link to="/">
        <li :class="{'header-bar-tabitem':true,'header-bar-tabitem-bgon':bgEnable,'header-bar-tabitem-active':currTab == '/'}">首页</li>
      </router-link>
      <router-link :to="item.url" v-for="(item,index) in navs" :key="index">
        <li :class="{'header-bar-tabitem':true,'header-bar-tabitem-bgon':bgEnable,'header-bar-tabitem-active':currTab == item.url}">{{item.name}}</li>
      </router-link>

    </ul> -->
    <div class="header-bar-right">
      <ClockBox />
      <!-- <a href="settings://">123</a> -->
    </div>
    
  </div>
</template>
 
<script>
import ClockBox from '@/components/ClockBox'


export default {
  name: "HeaderBar",
  components: {
    ClockBox
  },
  data() {
    return {
      bgEnable:false,
      navs:[
        {
        name:"首页",
        url:"/"
      },{
        name:"测试",
        url:"/test"
      }]
    };
  },
  computed:{
    currTab:function(){
      return this.$route.path;
    },
    clockBoxStat:function(){
      return this.$store.state.clockBoxStat;
    }
  },
  methods: {
    printPath(){
      console.log(this.$route.path)
    },
    handleScroll() {
      let scrollTop =
        window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
      // let offsetTop = document.querySelector("body").offsetTop;
      // console.log(scrollTop);
      // console.log(offsetTop);
      // console.log(this.$store.state.clockBoxStat);
      if (scrollTop) {
        this.bgEnable = true;
      } else{
        this.bgEnable = false;
      }
    },
    handleShowBg(){
      if(this.bgEnable){
        return ' header-bar-container-bgon'
      }else{
        return ''
      }
    },
    handleChangeFontColor(){
      if(this.bgEnable){
        return ' header-bar-tabitem-bgon'
      }else{
        return ''
      }
    }
  },
  
  mounted(){
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
  }
};
</script>

<style>

.header-bar-container {
  box-sizing: border-box;
  user-select: none;
  position: sticky;
  top: 0px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  z-index:1000;
  transition: all .6s ease;
  padding: 5px 10px;
  /* background: linear-gradient(var(--elem-color),transparent); */
  height: 64px;
}
.header-bar-container-bgon{
  background: var(--blur-color);
  backdrop-filter: blur(40px) saturate(120%);
  transition: all .05s ease;

  box-shadow: 0 0px 10px 4px #00000010;
}
.header-bar-container-bg-no-blur{
  background: var(--elem-color)!important;
  backdrop-filter: none;
}
.header-bar-tablist {
  display: flex;
  align-items: center;
  justify-content: center;
}
.header-bar-tabitem {
  flex-shrink:0;
  width: max-content;
  /* font-size: 15px; */
  color: var(--main-color);
  margin: 0 5px;
  padding: 8px 15px;
  font-weight: 400;
  border-radius: 25px;
  transition: all 0.2s ease;
}
.header-bar-tabitem-bgon {
  text-shadow: none;

}
.header-bar-tabitem:hover {
  /* color: #000; */
  /* font-size: 20px; */
  /* box-shadow: 0 2px 10px 2px #00000023; */
  /* font-weight: 600; */
  /* background: #ffffff65; */
  background: var(--accent-color-opa);
  color: #fff;
  /* backdrop-filter: blur(30px) saturate(180%); */
  text-shadow:none;
}
.header-bar-tabitem:active,
.header-bar-tabitem-active{
  /* font-size: 20px; */
  /* box-shadow: 0 2px 10px 2px #00000023; */
  /* font-weight: 600; */
  /* padding-right: 12px;
  padding-left: 12px; */
  
  /* background: #ffffff65; */
  background: var(--accent-color)!important;
  color: #fff;

  /* backdrop-filter: blur(30px) saturate(180%); */
  transition: all 0.2s ease;
  text-shadow:none;
}
.header-bar-right{
  display: flex;
}
</style>