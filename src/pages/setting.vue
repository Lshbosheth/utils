<template>
  <view class="body">
    <u--form labelPosition="left" :labelWidth="150" ref="uForm">
      <u-form-item
        v-for="(value, index) in infoList"
        label="配置名"
        prop="userInfo.configName"
        borderBottom
        ref="item1"
      >
        <u--input v-model="value.configName" border="none"></u--input>
        <view
          slot="right"
          style="display: flex; width: 100rpx; justify-content: space-between"
        >
          <u-icon name="plus-circle" size="50" @click="addValue"></u-icon>
          <u-icon
            name="minus-circle"
            size="50"
            @click="delValue(index)"
            v-if="index != 0"
          ></u-icon>
        </view>
      </u-form-item>
    </u--form>
    <view style="margin-top: 30rpx">
      <u-button
        :disabled="isDisabled"
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
export default {
  data() {
    return {
      isDisabled: false,
      infoList: [{ configName: "", configSort: 1 }],
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
      this.isDisabled = true;
      uni.request({
        url: "https://8kb7yj297854.vicp.fun/api/utils/updateConfig",
        method: "POST",
        data: { infoList: this.infoList},
        timeout: 6000,
        success: (res) => {
          this.isDisabled = false;
          this.$refs.uToast.show({
            icon: false,
            type: "success",
            message: "修改成功",
          });
        },
        fail: (err) => {
          this.isDisabled = false;
          this.$refs.uToast.show({
            icon: false,
            type: "error",
            message: err,
          });
        },
        complete: () => {},
      });
    },
    addValue() {
      this.infoList.push({
        configName: "",
        configSort: this.infoList.length + 1
      });
    },
    delValue(index) {
      this.infoList.splice(index, 1);
    },
  },
};
</script>

<style lang="scss" scoped>
.body {
  padding: 30rpx;
  .inputCel {
    display: flex;
  }
}
</style>
