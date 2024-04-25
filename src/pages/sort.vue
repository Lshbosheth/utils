<template>
  <view class="body">
    <view>
      <basic-drag v-model="infoList" :column="1" itemHeight="50px" itemKey="configName">
        <template #item="{element}">
          <view class="drag-item">{{ element.configName }}</view>
        </template>
      </basic-drag>
      <view style="margin-top: 10px;">
        拖拽后顺序：{{infoList.map(item=>item.configName).join('、')}}
      </view>
    </view>

    <view style="margin-top: 30rpx">
      <u-button
        type="primary"
        shape="circle"
        text="保存"
        @click="confirm"
      ></u-button>
    </view>
    <u-toast ref="uToast"></u-toast>

  </view>
</template>

<script>
import BasicDrag from '@/components/basic-drag/index.vue';
import { onLoad } from '@dcloudio/uni-app';
export default {
  components: { BasicDrag },
  data() {
    return {
      infoList: []
    };
  },
  onLoad() {
    uni.request({
      url: "https://8kb7yj297854.vicp.fun/api/utils/allConfig",
      method: "GET",
      timeout: 6000,
      success: (res) => {
        let infoList = res.data.data.sort((a, b) => a.configSort - b.configSort)
        this.infoList = infoList
      },
      complete: () => {},
    });
  },
  methods: {
    confirm() {
      console.log(this.infoList)
      const body = []
      this.infoList.forEach((e, index) => {
        e.configSort = index + 1
        body.push(e)
      })
      uni.request({
        url: "https://8kb7yj297854.vicp.fun/api/utils/updateConfig",
        method: "POST",
        data: { infoList: body},
        timeout: 6000,
        success: (res) => {
          this.$refs.uToast.show({
            icon: false,
            type: "success",
            message: "修改成功",
          });
        },
        fail: (err) => {
          this.$refs.uToast.show({
            icon: false,
            type: "error",
            message: err,
          });
        },
        complete: () => {},
      });
    },
  }
};
</script>

<style lang="scss" scoped>
.list-title {
  box-sizing: border-box;
  padding-left: 20px;
  margin-top: 20px;
  margin-bottom: 20px;
}
.body {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
