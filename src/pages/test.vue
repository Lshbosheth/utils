<template>
  <view>
    <u-upload
        :fileList="fileList1"
        @afterRead="afterRead"
        @delete="deletePic"
        name="1"
        multiple
        :maxCount="10"
    ></u-upload>

    <u-button text="月落" @click="submit"></u-button>

  </view>
</template>

<script>
export default {
  data() {
    return {
      fileList1: [],
    }
  },
  methods:{
    submit() {
      console.log(this.fileList1)
    },
    // 删除图片
    deletePic(event) {
      this[`fileList${event.name}`].splice(event.index, 1)
    },
    // 新增图片
    async afterRead(event) {
      console.log(event);
      // 当设置 multiple 为 true 时, file 为数组格式，否则为对象格式
      let lists = [].concat(event.file)
      let fileListLen = this[`fileList${event.name}`].length
      lists.map((item) => {
        this[`fileList${event.name}`].push({
          ...item,
          status: 'uploading',
          message: '上传中'
        })
      })
      for (let i = 0; i < lists.length; i++) {
        const result = await this.uploadFilePromise(lists[i].url)
        let item = this[`fileList${event.name}`][fileListLen]
        this[`fileList${event.name}`].splice(fileListLen, 1, Object.assign(item, {
          status: 'success',
          message: '',
          url: result.path
        }))
        fileListLen++
      }
    },
    uploadFilePromise(url) {
      return new Promise((resolve, reject) => {
        let a = uni.uploadFile({
          url: 'https://api.lshbosheth.cn/api/upload',
          filePath: url,
          name: 'file',
          success: (res) => {
            setTimeout(() => {
              console.log(res.data);
              resolve(JSON.parse(res.data).data)
            }, 1000)
          }
        });
      })
    },
  }

}
</script>

<style lang="scss">

</style>
