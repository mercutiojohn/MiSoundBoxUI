<template>
  <div class="big-clock">
    <transition name="trans-vert" type="out-in">
      <div class="big-clock-ticktick" v-show="clockWrapDisplay">
        <div class="ticktick-func">
          <i
            class="iconfont icon-chevron-down"
            @click="clockWrapDisplay = false"
          ></i>
            <span class="twelve-label">{{ clockPreferences.twelveLabel }}</span>
          <span class="small-time">
            {{ date }}
            </span>
          <span class="small-date">{{ month }} / {{ day }}</span>
          <span class="small-week">周{{ week }}</span>
        </div>
        <EmbedFrame
          url="https://www.dida365.com/webapp/#q/all/today"
          title="滴答清单"
          height="calc(100vh - 1px)"
          :hideExpand="true"
          :hideActionBar="true"
        />
      </div>
    </transition>
    <transition name="fade" type="out-in">
      <div id="big-clock-content" v-show="!clockWrapDisplay">
        <div
          id="big-clock-time"
          :class="{ 'big-clock-time-active': clockWrapDisplay }"
          v-on:click="changeWrapState('display')"
        >
          <span
            id="big-clock-time-content"
            :class="bgStatus ? 'bg-text-style' : '' + handleBgOn()"
          >
            {{ date }}
          </span>
          <span
            id="big-clock-twelve-label"
            v-if="clockPreferences.twelveFormat"
            :class="bgStatus ? 'bg-text-style' : '' + handleBgOn()"
          >
            {{ clockPreferences.twelveLabel }}
          </span>
        </div>
        <div class="date-content">
          <span>{{ month }}月{{ day }}日</span>
          <span>周{{ week }}</span>
        </div>
        <div class="entries">
          <div class="tomato-entry entry">
            <i class="iconfont icon-clock before"></i>
            <span class="entry-title"> 25:00 </span>
            <i class="iconfont icon-chevron-right"></i>
          </div>
          <div class="ticktick-entry entry" @click="clockWrapDisplay = true">
            <i class="iconfont icon-circle-check before"></i>
            <span class="entry-title"> 滴答清单 </span>
            <i class="iconfont icon-chevron-right"></i>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
import EmbedFrame from "@/components/EmbedFrame";
import ClockBox from "@/components/ClockBox";
import CalendarBox from "@/components/CalendarBox";
export default {
  props: ["bgEnable"],
  name: "BigClock",
  components: {
    EmbedFrame,
    ClockBox,
    CalendarBox,
  },
  data() {
    return {
      timer: "",
      date: "",
      clockPreferences: {
        twelveFormat: true,
        showSecond: false,
      },
      currentDate: new Date(),
      bgStatus: true,
      clockWrapDisplay: false,
      year: "",
      month: "",
      day: "",
      week: "",
    };
  },
  methods: {
    getTime() {
      var today = new Date();
      var h = today.getHours();
      var m = today.getMinutes();
      var s = today.getSeconds();
      // add a zero in front of numbers <10
      m = this.checkTime(m);
      s = this.checkTime(s);

      if (this.clockPreferences.twelveFormat) {
        this.clockPreferences.twelveLabel = h / 12 >= 1 ? "下午" : "上午";
        h = h == 12 ? h : h % 12;
      }
      var result = this.clockPreferences.showSecond
        ? h + ":" + m + ":" + s
        : h + ":" + m;
      this.date = result;
    },
    checkTime(i) {
      return i < 10 ? "0" + i : i;
    },
    generateDate(objD, option) {
      var str, colorhead, colorfoot;
      var yy = objD.getYear();
      if (yy < 1900) yy = yy + 1900;
      var MM = objD.getMonth() + 1;
      // if (MM == 0) MM = "一";
      // if (MM == 1) MM = "二";
      // if (MM == 2) MM = "三";
      // if (MM == 3) MM = "四";
      // if (MM == 4) MM = "五";
      // if (MM == 5) MM = "六";
      // if (MM == 6) MM = "七";
      // if (MM == 7) MM = "八";
      // if (MM == 8) MM = "九";
      // if (MM == 9) MM = "十";
      // if (MM == 10) MM = "十一";
      // if (MM == 11) MM = "十二";
      // if (MM < 10) MM = '0' + MM;
      var dd = objD.getDate();
      if (dd < 10) dd = "0" + dd;
      var ww = objD.getDay();
      if (ww == 0) ww = "日";
      if (ww == 1) ww = "一";
      if (ww == 2) ww = "二";
      if (ww == 3) ww = "三";
      if (ww == 4) ww = "四";
      if (ww == 5) ww = "五";
      if (ww == 6) ww = "六";
      if (option == "y") {
        str = yy;
      } else if (option == "m") {
        str = MM;
      } else if (option == "d") {
        str = dd;
      } else if (option == "w") {
        str = ww;
      } else {
        str = yy + "-" + MM + "-" + dd + " " + ww;
      }
      return str;
    },
    getDate() {
      var today;
      today = new Date();
      this.year = this.generateDate(today, "y");
      this.month = this.generateDate(today, "m");
      this.day = this.generateDate(today, "d");
      this.week = this.generateDate(today, "w");
    },
  },
  created() {
    // this.date = this.getTime();
    let _this = this;
    this.timer = setInterval(function () {
      _this.getTime();
      _this.getDate();
    }, 1000);
  },
  computed: {
    week: function () {
      function getYearWeek(date) {
        var date2 = new Date(date.getFullYear(), 0, 1);
        var day1 = date.getDay();
        if (day1 == 0) day1 = 7;
        var day2 = date2.getDay();
        if (day2 == 0) day2 = 7;
        var d = Math.round(
          (date.getTime() -
            date2.getTime() +
            (day2 - day1) * (24 * 60 * 60 * 1000)) /
            86400000
        );
        return Math.ceil(d / 7) + 1;
      }
      var start = new Date(2020, 2, 17);
      return getYearWeek(start);
    },
  },
};
</script>
<style lang="css" scoped>
.trans-vert-enter-active {
  transition: all 0.15s cubic-bezier(0.36, 0.13, 0.04, 1);
}

.trans-vert-leave-active {
  transition: all 0.1s cubic-bezier(0.43, 0.2, 0.86, 0.88);
  /* transition-delay: 200ms; */
}

.trans-vert-enter,
.trans-vert-leave-to {
  /* background: transparent; */
  transform: translateY(100vh);
  opacity: 0;
  /* backdrop-filter: blur(10px); */
}
:root {
  --wrap-gap: 10px;
}
#big-clock {
  width: 100%;
  height: 100vh;
  overflow: scroll;
  user-select: none;
}
#big-clock-content {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--elem-color-dark);
}
#big-clock-time {
  display: flex;
  align-items: baseline;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
}
.big-clock-time-active {
}
#big-clock-time-content {
  cursor: pointer;
  font-size: calc(95vmin - 130px);
  line-height: calc(95vmin - 130px);
  /* font-weight: 800; */
  font-family: "Mitype2018-90", Politica, -apple-system, SF UI Text,
    "SF Pro Display";
  /* transform:translateY(-15px); */
}
#big-clock-time-content,
#big-clock-twelve-label,
#big-clock-week {
  color: var(--main-color-dark);
  margin-left: 5px;
  /* text-shadow: 0 5px 10px #00000083; */
}

.big-clock-noshadow {
  text-shadow: none !important;
  transition: all 0.3s ease;
}
#big-clock-twelve-label {
  font-size: 25px;
  background: var(--bg-color-dark);
  padding: 3px 6px;
  border-radius: 10px;
  /* font-weight: 400; */
  color: var(--inactive-color-dark);
}
.entries {
  color: var(--main-color-dark);
  display: flex;
}
.entries .entry {
  padding: 10px 5px;
  background: var(--bg-color-dark);
  margin: 0 10px;
  display: flex;
  align-items: center;
  border-radius: 20px;
  transition: background-color 0.2s ease;
}
.entries .entry:active {
  background: var(--accent-color);
}
.entries .entry .iconfont {
  font-size: 20px;
  margin: 0 4px;
}
.entries .entry .before {
  margin-right: 6px;
}
.big-clock-ticktick {
  background: #fff;
  height: 100vh;
  width: 100vw;
  display: flex;
}
.ticktick-func {
  width: 100px;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  margin: 10px 0 0px 10px;
  /* background: var(--second-assist-color); */
}
.ticktick-func span {
  /* color:var(--main-color) */
  font-size: 18px;
}
.ticktick-func .small-time {
  margin-bottom: 20px;
  border-bottom: 1px solid #00000023;
  font-size: 30px;
  /* font-weight: 300; */
}
.ticktick-func .small-date{
  font-size: 20px;
  font-weight: 800;
}
.ticktick-func .twelve-label {
  font-size: 15px;
}
.ticktick-func .small-week {
  font-size: 30px;
}
.ticktick-func i {
  display: block;
  font-size: 30px;
  margin-bottom: 10px;
  /* background: #a00; */
  padding: 20px;
  box-sizing: border-box;
  border-radius: 15px;
}
.ticktick-func i:active {
  background: var(--second-assist-color);
}
.date-content {
  color: var(--inactive-color-dark);
  /* transform:translateY(-20px); */
  font-size: 25px;
  padding: 10px 0;
  /* font-weight: 300;*/
}
</style>
