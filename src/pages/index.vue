<template>
  <view class="body">
    <u-list>
      <u-list-item v-for="(item, index) in list" :key="index">
        <view class="celItem" @click="clickCel(item)">
          {{ item.title }}
        </view>
      </u-list-item>
    </u-list>
  </view>
</template>

<script>
export default {
  data() {
    return {
      list: [
        { title: "记录", url: "/pages/info" },
        { title: "拍照", url: "/pages/photo" },
        { title: "表单", url: "/pages/test" },
        { title: "设置", url: "/pages/setting" },
      ],
      loading: false,
      finished: true,
    };
  },
  methods: {
    clickCel(e) {
      uni.navigateTo({
        url: e.url,
      });
    },
    onLoad() {
    },
  },
};
</script>

<style scoped>
.body {
  padding-top: 160rpx;
}
.celItem {
  padding: 30rpx 0;
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom: 1px solid #eeeeee;
}
</style>
