<template>
  <div id="countdown">
    <transition name="fade">
      <div class="countdown-choose" v-if="expand" @click="expand = false">
        <div
          class="countdown-choose-item"
          v-for="(item, i) in dates"
          :key="i"
          @click="(index = i), (expand = false)"
        >
          {{ item.name }}
        </div>
      </div>
    </transition>
    <transition name="fade">
      <div class="countdown-expanded" v-if="!expand">
        <div class="countdown-days">
          {{ days }}<span class="countDown-label">天</span>
        </div>
        <div class="countdown-title" @click="expand = true">
          {{
            year + Number(dates[index].year_type ? dates[index].year_type : 0)
          }}
          {{ dates[index].name }}
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "Countdown",
  data() {
    return {
      year: "",
      expand: false,
      index: 0,
      dates: [
        {
          name: "考研",
          date: "Dec 22",
          year_type: "1",
        },
        {
          name: "公考",
          date: "Mar 27",
        },
        {
          name: "高考",
          date: "Jun 7",
        },
        {
          name: "上半年四六级",
          date: "Jun 12",
        },
        {
          name: "下半年四六级",
          date: "Dec 11",
        },
      ],
      selfName: "",
      selfDate: "",
    };
  },
  watch: {
    index(value) {
        localStorage.countdown_index = value;
    },
  },
  computed: {
    days: function () {
      var currYear = new Date().getFullYear();
      this.year = currYear;
      var today = new Date(); //当前时间
      var h = today.getHours();
      var m = today.getMinutes();
      var s = today.getSeconds();
      var stopTime = new Date(
        this.dates[this.index].date + " " + currYear + " 00:00:00"
      ); //结束时间
      var stopH = stopTime.getHours();
      var stopM = stopTime.getMinutes();
      var stopS = stopTime.getSeconds();
      var shenyu = stopTime.getTime() - today.getTime(); //倒计时毫秒数
      var shengyuD = parseInt(shenyu / (60 * 60 * 24 * 1000)); //转换为天
      var D = parseInt(shenyu) - parseInt(shengyuD * 60 * 60 * 24 * 1000); //除去天的毫秒数
      var shengyuH = parseInt(D / (60 * 60 * 1000)); //除去天的毫秒数转换成小时
      var H = D - shengyuH * 60 * 60 * 1000; //除去天、小时的毫秒数
      var shengyuM = parseInt(H / (60 * 1000)); //除去天的毫秒数转换成分钟
      var M = H - shengyuM * 60 * 1000; //除去天、小时、分的毫秒数
      var S = parseInt(
        (shenyu -
          shengyuD * 60 * 60 * 24 * 1000 -
          shengyuH * 60 * 60 * 1000 -
          shengyuM * 60 * 1000) /
          1000
      ); //除去天、小时、分的毫秒数转化为秒
      if (shengyuD < 0) {
        shengyuD =
          shengyuD +
          365 +
          Number(
            currYear % 4 == 0 && (currYear % 100 != 0 || currYear % 400 == 0)
          );
        this.year = currYear + 1;
      }
      return shengyuD;
    },
  },
  methods: {
    changeExpand() {
      if (this.expand == true) {
        this.expand = false;
      } else {
        this.expand = true;
      }
      // window.localStorage.setItem("kaoyan_status",this.expand);
    },
  },
  beforeCreate() {},
  created() {
    this.timer = new Date();
  },
  mounted() {
    this.year = new Date().getFullYear();
    if (localStorage.countdown_index)
      this.index = Number(localStorage.countdown_index);
  },
};
</script>

<style>
#countdown {
  /* color: var(--main-color); */
  /* display: flex; */
  /* flex-direction: column; */
  transition: all 0.2s ease;
  background: linear-gradient(-45deg, rgb(247, 21, 115), rgb(132, 0, 255));
  background: linear-gradient(-45deg, rgb(247, 21, 21), rgb(111, 0, 255));
  /* background: linear-gradient(-45deg, rgb(21, 247, 198) , rgb(190, 204, 0) ); */

  color: #fff;
  height: 100%;
}
.countdown-days {
  font-size: 80px;
  font-family: Mitype2018-90, Mitype2018-60, Politica, Palatino, Caecilia,
    Bookerly;
  padding: 0 0 10px 0;
}
.countdown-expand {
  /* position:relative;
  right: 20px;
  top: 20px; */
  padding: 3px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  cursor: pointer;
  flex-grow: 0;
  /* height: min-content; */
}
.countdown-expand:hover {
  background: var(--first-assist-color);
}
.countdown-expand:active {
  background: var(--first-assist-color);
}
.countdown-header {
  box-sizing: border-box;
  width: 100%;
  display: flex;
  /* flex: 1; */
  padding: 8px 20px;
  align-items: center;
  justify-content: flex-end;
  flex-wrap: wrap-reverse;
}
.countdown-closed {
  width: 100%;
  display: flex;
  flex: 1;
  align-items: center;
  transition: all 0.5s ease;
}
.countdown-closed-hidden {
  opacity: 0;
}
.countdown-expanded {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  padding: 20px;
  height: 100%;
  width: 100%;
  box-sizing: border-box;
  overflow: hidden;
  /* max-height: 200px; */
  /* transition: all 0.3s ease; */
}
.countdown-expanded-hidden {
  max-height: 0;
  padding: 0;
  opacity: 0;
}
/* .countdown-closed-days {
  font-size: 25px;
  font-family: Mitype2018-90, Politica, Mitype2018-60, Palatino, Caecilia,
    Bookerly;
  margin: 0 5px 5px;
} */
.countDown-label {
  font-size: 30px;
}

.countdown-choose {
  height: 100%;
  display: flex;
  flex-direction: column;
  padding: 20px;
  background: var(--elem-color);
  color: var(--main-color);
  width: 100%;
  box-sizing: border-box;
}
.countdown-choose-item {
  padding: 10px;
  border-bottom: 1px solid #00000013;
  cursor: pointer;
  /* border-radius: 10px; */
  transition:all .2s ease;
}
.countdown-choose-item:active{
  background: var(--first-assist-color);

}
.countdown-choose-item:last-child {
  border-bottom: none;
}
.countdown-title {
  font-size: 20px;
  padding: 3px;
  cursor: pointer;
}
.countdown-title:active {
  border-radius: 5px;
  background: #00000034;
}
</style>