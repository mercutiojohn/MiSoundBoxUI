<template>
  <div class="embed">
    <div class="embed-bottom-bar" v-if="!hideActionBar">
      <span class="embed-title">{{ title }}</span>
      <div class="embed-icon-group">
        <a :href="url" target="_blank">
          <div class="embed-icon">
            <i
              :class="{
                iconfont: true,
                'icon-external-link': true,
                'left-info-icon': true,
              }"
            ></i>
          </div>
        </a>
        <div class="embed-icon" @click="changeWrapState()" v-if="!hideExpand">
          <i
            :class="{
              iconfont: true,
              'icon-chevron-up': expand,
              'icon-chevron-down': !expand,
              'left-info-icon': true,
            }"
          ></i>
        </div>
      </div>
    </div>
    <iframe
      :class="{ 'embed-frame': true, 'embed-frame-hidden': !expand&&!hideExpand }"
      :style="{ 'height': height, 'zoom':zoom }"
      :src="url"
      frameborder="0"
      allow="autoplay *; encrypted-media *; geolocation; microphone; camera"
      sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-storage-access-by-user-activation allow-top-navigation-by-user-activation"
      scrolling="auto"
    >
    </iframe>
  </div>
</template>

<script>
export default {
  name: "EmbedFrame",
  props: {
    url: String,
    title: String,
    height: {
      type: String,
      default: '200px'
    },
    zoom: {
      type: String,
      default: '1'
    },
    hideExpand: {
      type: Boolean,
      default: false
    },
    hideActionBar: {
      type:Boolean,
      default:false
    }
  },
  data() {
    return {
      expand: true,
    };
  },
  methods: {
    changeWrapState() {
      if (this.expand) this.expand = false;
      else this.expand = true;
    },
  },
};
</script>

<style>
.embed {
  display: flex;
  flex-direction: column;
  width: 100%;
}
.embed-frame {
  width: 100%;
  /* height: 500px; */
  display: block;
  transition: all 0.2s ease;
}
.embed-frame-hidden {
    opacity: 0;
  height: 0!important;
}
.embed-bottom-bar {
  zoom:1.1;
  box-sizing: border-box;
  width: 100%;
  display: flex;
  /* flex: 1; */
  padding: 8px 20px;
  align-items: center;
  justify-content: space-between;
  color: var(--main-color);
}
.embed-icon-group {
  display: flex;
}
.embed-icon {
  color: var(--main-color) !important;

  padding: 3px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  cursor: pointer;
  flex-grow: 0;
  height: min-content;
}
.embed-icon:last-child{
    margin-left: 10px;
}
.embed-icon:hover {
  background: var(--first-assist-color);
}
.embed-icon:active {
  background: var(--first-assist-color);
}
</style>