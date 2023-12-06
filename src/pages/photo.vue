<template>
  <view class="body">
    <view class="btnView">
      <view class="btnCel">
        <u-button type="primary" shape="circle" text="选择地点" @click="chooseLocation"></u-button>
      </view>
      <view class="btnCel">
        <u-button class="btnCel" type="primary" shape="circle" text="拍照" @click="photo"></u-button>
      </view>
    </view>
    <view class="imgView">
      <image show-menu-by-longpress mode="widthFix" :src="src"></image>
    </view>
    <view style="position: absolute;top: -999999px;">
      <view>
        <canvas :style="{'width':width,'height': height}" canvas-id="firstCanvas"></canvas>
      </view>
    </view>
  </view>
</template>

<script>
import dayjs from "dayjs";

export default {
  data() {
    return {
      color:'#000',
      addrInfo: {},
      src: '',
      width: '',
      height: ''
    }
  },
  onLoad() {

  },
  methods: {
    photo() {
      let that = this;
      uni.chooseImage({
        count: 1,
        success(res) {
          console.log(res);
          uni.getImageInfo({
            src: res.tempFilePaths[0],
            success: (ress) => {
              console.log(ress);
              that.width = ress.width / 3 + 'px';
              that.height = ress.height / 3 + 'px';
              let ctx = uni.createCanvasContext('firstCanvas');
              ctx.drawImage(res.tempFilePaths[0], 0, 0, ress.width / 3, ress.height / 3)
              ctx.setFontSize(18)
              ctx.setFillStyle('#000')
              let date = dayjs(new Date()).format('YYYY-MM-DD  HH:mm:ss')
              console.log(ress.height / 3);
              ctx.fillText(`时间：${date}`, 10, (ress.height / 3) - 70)
              ctx.fillText(`地点：${that.addrInfo.address}`, 10, (ress.height / 3) - 50)
              ctx.fillText(`经纬度：${that.addrInfo.longitude},${that.addrInfo.latitude}`, 10, (ress.height / 3) - 30)
              ctx.draw(false, () => {
                setTimeout(() => {
                  uni.canvasToTempFilePath({
                    canvasId: 'firstCanvas',
                    success: (res1) => {
                      console.log(res1);
                      that.src = res1.tempFilePath;
                    }
                  });
                }, 1000);
              });
            }
          })
        }
      });
    },
    chooseLocation() {
      const that = this
      uni.chooseLocation({
        success: function (res) {
          that.addrInfo = res
        }
      });
    }
  }
}
</script>

<style scoped lang="scss">
.body {
  padding: 60rpx 30rpx;

  .btnView {
    display: flex;
    justify-content: space-between;

    .btnCel {
      width: 40%;
      padding: 0 30rpx;
    }
  }

  .imgView {
    margin-top: 30rpx;
    display: flex;
    align-items: center;
    justify-content: center;
  }

}
</style>
