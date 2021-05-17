<template>
  <div class="big-clock">
    <transition name="trans" type="out-in">
<div class="big-clock-ticktick" v-show="clockWrapDisplay" >
      <div class="ticktick-func" @click="clockWrapDisplay = false">
        <i class="iconfont icon-chevron-down"></i>
          <!-- <clock-box />
          <calendar-box /> -->

      </div>
      <EmbedFrame url="https://www.dida365.com/webapp/#q/all/today"
          title="滴答清单"
          height="calc(100vh - 1px)"
          :hideExpand="true"
          :hideActionBar="true"/>
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
import ClockBox from '@/components/ClockBox.vue';
import CalendarBox from '@/components/CalendarBox.vue';
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
      return result;
    },
    checkTime(i) {
      return i < 10 ? "0" + i : i;
    },
  },
  created() {
    // this.date = this.getTime();
    let _this = this;
    this.timer = setInterval(function () {
      _this.date = _this.getTime();
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
<style>
.trans-enter-active {
  transition: all 0.15s cubic-bezier(0.36, 0.13, 0.04, 1);
}

.trans-leave-active {
  transition: all 0.1s cubic-bezier(0.43, 0.2, 0.86, 0.88);
  /* transition-delay: 200ms; */
}

.trans-enter,
.trans-leave-to {
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
  font-size: calc(95vmin - 100px);
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
  font-size: 40px;
  font-weight: 800;
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
  transition: background-color .2s ease;
}
.entries .entry:active{
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
.ticktick-func{
  width: 100px;
  height: 100vh;
  background: var(--second-assist-color);
}
.ticktick-func i{
  display: block;
  font-size: 30px;
  margin: 20px 0 10px 20px;
  /* background: #a00; */
  padding: 15px;
  box-sizing: border-box;
  border-radius: 15px;
}
.ticktick-func i:active{
  background: var(--second-assist-color);
}
</style>
