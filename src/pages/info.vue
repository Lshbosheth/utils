<template>
  <view class="body">

    <u--form
        labelPosition="left"
        :model="info"
        :labelWidth="150"
        ref="uForm"
    >
      <u-form-item
          label="日期"
          prop="userInfo.name"
          borderBottom
      >
        <u--input
            v-model="info.date"
            @focus="showCalendar = true"
            border="none"
        ></u--input>
      </u-form-item>

      <u-form-item
          label="地点"
          prop="userInfo.addr"
          borderBottom
      >
        <u--input
            v-model="info.addr"
            border="none"
        ></u--input>
      </u-form-item>

      <u-form-item
          label="用车人"
          prop="userInfo.personal"
          borderBottom
      >
        <u--input
            v-model="info.personal"
            border="none"
        ></u--input>
      </u-form-item>

      <u-form-item
          label="车型"
          prop="userInfo.type"
          borderBottom
      >
        <u--input
            v-model="info.type"
            border="none"
        ></u--input>
      </u-form-item>

      <u-form-item
          label="车号"
          prop="userInfo.carNo"
          @click="showCarKeyboard = true"
          borderBottom
      >
        <u--input
            disabled
            disabledColor="#fff"
            v-model="info.carNo"
            border="none"
        ></u--input>
      </u-form-item>

      <u-form-item
          label="司机"
          prop="userInfo.driver"
          borderBottom
      >
        <u--input
            v-model="info.driver"
            border="none"
        ></u--input>
      </u-form-item>

      <u-form-item
          label="施工时间"
          prop="userInfo.addr"
          borderBottom
      >
        <u--input
            v-model="info.time"
            border="none"
        ></u--input>
      </u-form-item>

      <u-form-item
          label="施工内容"
          prop="userInfo.desc"
          borderBottom
      >
        <u--input
            v-model="info.desc"
            border="none"
        ></u--input>
      </u-form-item>
    </u--form>
    <view style="display: flex;justify-content: center;margin-top: 60rpx">
      <u-button type="primary" shape="circle" text="复制" @click="copy"></u-button>
<!--      <u-button type="primary" shape="circle" text="分享" @click="share"></u-button>-->
    </view>

    <view class="infoBox">
      日期: {{info.date}}<br/>
      地点: {{info.addr}}<br/>
      用车人: {{info.personal}}<br/>
      车型: {{info.type}}<br/>
      车号: {{info.carNo}}<br/>
      司机: {{info.driver}}<br/>
      施工时间: {{info.time}}<br/>
      施工内容: {{info.desc}}<br/>
    </view>
    <u-keyboard ref="uKeyboard" @change="valChange" @backspace="backspace"  mode="car" :show="showCarKeyboard" @cancel="showCarKeyboard = false"  @confirm="showCarKeyboard = false"></u-keyboard>
    <u-calendar :show="showCalendar" @close="showCalendar = false" @confirm="confirmCalendar"></u-calendar>
  </view>
</template>
<script>
import dayjs from 'dayjs'
export default {
  data() {
    return {
      showCalendar: false,
      showCarKeyboard: false,
      info: {
        date: dayjs(new Date()).format('YYYY年MM月DD日'),
        addr: '',
        personal: '',
        driver: '',
        type: '',
        carNo: '',
        time: '',
        desc: ''
      },
      infoList: []
    }
  },
  onLoad() {
    uni.request({
      url: "https://api.lshbosheth.cn/api/utils/allConfig",
      method: "GET",
      timeout: 6000,
      success: (res) => {
        this.infoList = res.data.data.info.split(',')
        console.log(this.infoList);
      }
    });
  },
  mounted() {
  },
  methods: {
    share() {
      uni.share({
        provider: "weixin",
        scene: "WXSceneSession",
        type: 1,
        summary: `日期: ${this.info.date} \n地点: ${this.info.addr}\n用车人: ${this.info.personal} \n车型: ${this.info.type}\n车号: ${this.info.carNo}\n司机: ${this.info.driver}\n施工时间: ${this.info.time}\n施工内容: ${this.info.desc}`,
        success: function (res) {
          console.log("success:" + JSON.stringify(res));
        },
        fail: function (err) {
          console.log("fail:" + JSON.stringify(err));
        }
      });
    },
    valChange(val) {
      this.info.carNo += val;
    },
    // 退格键被点击
    backspace() {
      if(this.info.carNo.length) this.info.carNo = this.info.carNo.substr(0, this.info.carNo.length - 1);
    },
    copy() {
      uni.setClipboardData({
        data: `日期: ${this.info.date} \n地点: ${this.info.addr}\n用车人: ${this.info.personal} \n车型: ${this.info.type}\n车号: ${this.info.carNo}\n施工时间: ${this.info.time}\n施工内容: ${this.info.desc}`
      });
    },
    confirmCalendar(e) {
      this.info.date = dayjs(e).format('YYYY年MM月DD')
      this.showCalendar = false
    },
    onSubmit() {
      console.log(this.info)
    }
  }
}
</script>


<style scoped>
.body {
  padding: 60rpx 30rpx;
}
.infoBox {
  border: 1px solid #eee;
  margin: 30rpx;
  padding: 25rpx;
  border-radius: 20rpx;
}
</style>
