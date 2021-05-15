<template>
  <div id="big-clock">
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
    <!-- <div class="tomato-entry">
      <i class="iconfont icon-chevron-right"></i>
      <span class="entry-title">
        25:00
      </span>
      <i class="iconfont icon-chevron-right"></i>
    </div> -->
  </div>
</template>
<script>
import EmbedFrame from "@/components/EmbedFrame";
export default {
  props: ["bgEnable"],
  name: "BigClock",
  components: {
    EmbedFrame,
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
:root {
  --wrap-gap: 10px;
}
#big-clock {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
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
  color: var(--main-color);
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
</style>
