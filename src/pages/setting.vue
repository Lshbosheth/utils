<template>
  <view class="body">
    <u--form labelPosition="left" :labelWidth="150" ref="uForm">
      <u-form-item
        v-for="(value, index) in valueList"
        label="配置名"
        prop="userInfo.name"
        borderBottom
        ref="item1"
      >
        <u--input v-model="value.name" border="none"></u--input>
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
      valueList: [{ name: "" }],
    };
  },
  onLoad() {
    uni.request({
      url: "https://api.lshbosheth.cn/api/utils/allConfig",
      method: "GET",
      timeout: 6000,
      success: (res) => {
        let infoList = res.data.data.info.split(',')
        let valueList = []
        infoList.forEach(e => {
          valueList.push({
            name: e
          })
        })
        this.valueList = valueList
      },
      complete: () => {},
    });
  },
  methods: {
    confirm() {
      this.isDisabled = true;
      console.log(this.valueList);
      let havNameList = this.valueList.filter((e) => !!e.name);
      let requestBody = havNameList.map((e) => e.name).join(",");
      console.log(requestBody);
      uni.request({
        url: "https://api.lshbosheth.cn/api/utils/updateConfig",
        method: "PUT",
        data: { configName: "info", configValue: requestBody },
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
      this.valueList.push({
        name: "",
      });
    },
    delValue(index) {
      this.valueList.splice(index, 1);
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
