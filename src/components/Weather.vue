<template>
  <div class="weather">
    <div class="weather-content" v-if="!citySelect">
      <span class="city" @click="changeSelect">{{ city }}</span>
      <div class="tmp-n-brief">
        <span class="temprature">{{ tmpNew }}°</span>
        <span class="brief">{{ briefNew }}</span>
      </div>
      <div class="air-info">
        <span class="aqi">{{ aqi }}</span>
        <span class="level">{{ aqiLevel }}</span>
      </div>
      <div class="warning-info">
        <div class="info-item" v-for="(item, index) in warning" :key="index">
          <div class="label">
            <span class="type">{{ item.type }}</span>
            <span class="level">{{ item.level }}</span>
            <span>预警</span>
          </div>
        </div>
      </div>
    </div>

    <div class="change-city" v-if="citySelect">
      <span class="title">切换城市</span>
      <input type="text" v-model="cityTemp" />
      <div class="options">
        <button @click="changeSelect">确定</button>
        <button @click="cancelSelect">取消</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "weather",
  components: {},
  data() {
    return {
      city: "济南",
      tmpNew: "",
      briefNew: "",
      tmpOld: "",
      briefOld: "",
      resOld: "",
      resWeatherContent: "",
      resAirContent: "",
      resGeo: "",
      citySelect: false,
      cityTemp: "",
      aqi: "",
      aqiLevel: "",
      warning: [
        {
          id: "1010601202006202220000501",
          pubTime: "2020-06-20T22:20+08:00",
          title: "吉林省长春市气象台发布大风蓝色预警",
          startTime: "",
          endTime: "",
          status: "预警中",
          level: "蓝色",
          type: "大风",
          text:
            "长春市气象台2020年6月20日22时20分发布大风蓝色预警信号:预计未来24小时内，我市有5-6级西南风，瞬间风力可达7-8级，市应急管理局、市气象局联合提醒，注意做好防范。（预警信息来源：国家预警信息发布中心）",
        },
        {
          id: "1010601202006202220000501",
          pubTime: "2020-06-20T22:20+08:00",
          title: "吉林省长春市气象台发布大风蓝色预警",
          startTime: "",
          endTime: "",
          status: "预警中",
          level: "红色",
          type: "暴雨",
          text:
            "长春市气象台2020年6月20日22时20分发布大风蓝色预警信号:预计未来24小时内，我市有5-6级西南风，瞬间风力可达7-8级，市应急管理局、市气象局联合提醒，注意做好防范。（预警信息来源：国家预警信息发布中心）",
        },
      ],
    };
  },
  computed: {},
  watch: {},
  methods: {
    searchWeather: async function () {
      //注意：这里有 async 用来完成异步操作
      //由于调用api是异步操作
      //在请求的时候需要用async+await让它同步，否则数据不好取出
      //如果没有await返回的是一个Promise 对象，我学术短浅，暂时没学到，不会取

      let key = "0ae07031c52c4d448cb26af87903d648"; //引号中放入前面保存的key

      //方法一：老的API
      //老的api
      //这里的引号是tab键上面的引号，主要作用是方便数据拼接
      // let httpUrlOld = `https://free-api.heweather.net/s6/weather/now?location=${this.city}&key=${key}`;
      // let resOld = await fetch(httpUrlOld); //请求数据
      // let resultOld = await resOld.json(); //将请求的数据转换为json数据类型
      // let nowOld = resultOld.HeWeather6[0].now; //获取到json数据中的实况天气
      // console.log(nowOld);
      // this.tmpOld = nowOld.tmp; //获取到当前温度
      // this.briefOld = nowOld.cond_txt; //获取到当前天气
      // this.resOld = resultOld;

      //方法二：新的API
      //获取城市的ID
      let locUrl = `https://geoapi.qweather.com/v2/city/lookup?location=${this.city}&key=${key}`;
      let locRes = await fetch(locUrl);
      let locResult = await locRes.json();
      console.log(locResult);
      let id = locResult.location[0].id;
      this.resGeo = locResult;
      //根据城市id获取具体的天气
      let httpUrl1 = `https://devapi.qweather.com/v7/weather/now?location=${id}&key=${key}`;
      let weatherRes = await fetch(httpUrl1);
      let weatherResult = await weatherRes.json();
      console.log(weatherResult);
      this.resWeatherContent = weatherResult;
      let nowWeather = weatherResult.now;
      this.tmpNew = nowWeather.temp;
      this.briefNew = nowWeather.text;
      //根据城市id获取空气指数
      let airUrl = `https://devapi.qweather.com/v7/air/now?location=${id}&key=${key}`;
      let airRes = await fetch(airUrl);
      let airResult = await airRes.json();
      console.log(airResult);
      this.resAirContent = airResult;
      let nowAir = airResult.now;
      this.aqi = nowAir.aqi;
      this.aqiLevel = nowAir.category;
      //根据城市id获取预警信息
      let warnUrl = `https://devapi.qweather.com/v7/warning/now?location=${id}&key=${key}`;
      let warnRes = await fetch(warnUrl);
      let warnResult = await warnRes.json();
      console.log(warnResult);
      // this.warning = warnResult.warning;
    },
    changeSelect() {
      this.citySelect
        ? ((this.city = this.cityTemp),
          this.searchWeather(),
          (this.citySelect = false))
        : ((this.cityTemp = this.city), (this.citySelect = true));
    },
    cancelSelect() {
      this.citySelect = false;
    },
  },
  created() {},
  mounted() {
    this.searchWeather();
  },
  beforeDestroy() {},
};
</script>

<style>
.weather {
  width: 100%;
  height: 100%;
  background: linear-gradient(45deg, rgb(0, 140, 255), rgb(36, 211, 255));
}
.weather-content {
  padding: 20px;
  /* color: var(--main-color); */
  color: #fff;
}
.city {
}
.tmp-n-brief {
  /* display: flex; */
  /* flex-direction: column; */
}
.tmp-n-brief > .temprature {
  font-size: 80px;
  font-family: Mitype2018-90, Mitype2018-60, Politica, Palatino, Caecilia,
    Bookerly;
  /* padding: 0 0 10px 0; */
}
.tmp-n-brief > .brief {
  font-size: 25px;
}
.change-city {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: space-evenly;
  padding: 20px;
  height: 100%;
}
.change-city > .options {
  display: flex;
  justify-content: space-evenly;
}
.air-info {
  display: flex;
  width: max-content;
  background: rgb(0, 190, 127);
  border-radius: 5px;
  padding: 2px 5px;
  align-items: center;
  /* justify-content: baseline; */
}
.air-info > .aqi {
  font-family: Mitype2018-90;
  font-size: 20px;
  padding-right: 5px;
}
.info-item >.label>.level{

}
.info-item > .label{
  display: flex;
  width: max-content;
  background: rgb(0, 190, 127);
  border-radius: 5px;
  padding: 2px 5px;
  align-items: center;
margin: 2px 0;
}
</style>