<template>
  <view class="body">
    <u-button
        type="primary"
        shape="circle"
        text="订阅消息"
        @click="dingyue"
    ></u-button>
    <u-button
        type="primary"
        shape="circle"
        text="登录"
        @click="dl"
    ></u-button>
    <u-list>
      <u-list-item v-for="(item, index) in list" :key="index">
        <view class="celItem" @click="clickCel(item)">
          {{ item.title }}
        </view>
      </u-list-item>
    </u-list>

    <u-toast ref="uToast"></u-toast>
    <u-modal showCancelButton @confirm="show =false" cancelText="复制" @cancel="copy" :closeOnClickOverlay="true" :show="show" title="openID" :content='content'></u-modal>

  </view>
</template>

<script>
export default {
  data() {
    return {
      show: false,
      content: '',
      list: [
        { title: "记录", url: "/pages/info" },
        { title: "拍照", url: "/pages/photo" },
        { title: "表单", url: "/pages/test" },
        { title: "设置", url: "/pages/setting" },
        { title: "排序", url: "/pages/sort" },
      ],
      loading: false,
      finished: true,
    };
  },
  onLoad() {
  },
  methods: {
	  copy() {
		  uni.setClipboardData({
		    data: this.content
		  });
		  this.show = false
	  },
    dl() {
      uni.login({
        provider: 'weixin', //使用微信登录
        onlyAuthorize: true,
        success: (loginRes)=> {
          uni.request({
            url: "https://api.lshbosheth.cn/api/wechat/openID?code=" + loginRes.code,
            method: "GET",
            timeout: 6000,
            success: (res) => {
              console.log(res)
              this.show = true
              this.content = res.data.data
              // this.$refs.uToast.show({
              //   icon: false,
              //   type: "success",
              //   message: res.data.data
              // });
            }
          });
        }
      });
    },
    dingyue() {
      uni.requestSubscribeMessage({
        tmplIds: ['9ODFxKcrCNDP7kLdgY8lRPtMpMEVCK9ihUuW6kWdnu0', 'FOhSugSZ11ebVdw0a89HDEJ-kMKFp7DbCcPEgTNzv94'],
        success (res) {
          console.log(res)
        }
      })
    },
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
