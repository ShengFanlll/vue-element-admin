<template>
  <el-form ref="dataForm" :rules="rules" :model="temp" label-position="left" label-width="100px"
           style="width: 400px; margin-left:50px;"
  >
    <el-form-item label="开启活动1">
      <el-switch
        v-model="temp.value1"
        active-value="1"
        inactive-value="0"
      />
      <span style="color: red; font-size: 8px; margin-left: 10px">*草莓限时打8折</span>
    </el-form-item>
    <el-form-item label="开启活动2">
      <el-switch
        v-model="temp.value2"
        active-value="1"
        inactive-value="0"
      />
      <span style="color: red; font-size: 8px; margin-left: 10px">*购物满100减10快</span>
    </el-form-item>

    <el-form-item label="苹果" prop="appleQty">
      <el-input v-model="temp.appleQty"/>
      <span style="font-size: 8px">8元每斤</span>
    </el-form-item>
    <el-form-item label="草莓" prop="strawberryQty">
      <el-input v-model="temp.strawberryQty"/>
      <span style="font-size: 8px">13元每斤</span>
    </el-form-item>
    <el-form-item label="芒果" prop="mangoQty">
      <el-input v-model="temp.mangoQty"/>
      <span style="font-size: 8px">20元每斤</span>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm(temp)">计算总价</el-button>
    </el-form-item>
    <el-form-item label="总价" prop="sumPrice">
      <el-input v-model="temp.sumPrice"/>
    </el-form-item>
  </el-form>
</template>

<script>
import axios from 'axios'
import Vue from 'vue'

Vue.prototype.$http = axios

export default {
  name: 'CountPriceDemo',
  components: {},
  data() {
    return {
      temp: {
        value1: '0',
        value2: '0',
        appleQty: '0',
        strawberryQty: '0',
        mangoQty: '0',
        sumPrice: '0'
      },
      rules: {
        appleQty: [{
          required: true,
          pattern: /^[+]?(0|([1-9]\d*))?$/,
          message: '请输入大于等于0的整数',
          trigger: 'change'
        }],
        strawberryQty: [{
          required: true,
          pattern: /^[+]?(0|([1-9]\d*))?$/,
          message: '请输入大于等于0的整数',
          trigger: 'change'
        }],
        mangoQty: [{
          required: true,
          pattern: /^[+]?(0|([1-9]\d*))?$/,
          message: '请输入大于等于0的整数',
          trigger: 'change'
        }]
      }
    }
  },
  methods: {
    submitForm(data) {
      this.$refs.dataForm.validate((valid) => {
        if (valid) {
          const urlStr = 'http://127.0.0.1:8009' + '/count/getPrice'
          // const params = {
          //   value1: this.temp.value1,
          //   value2: this.temp.value2,
          //   appleQty: this.temp.appleQty,
          //   strawberryQty: this.temp.appleQty,
          //   mangoQty: this.temp.mangoQty
          // }
          // this.$http.post(url, params).then((res) => {
          //   success(res)
          // }, (res) => {
          //   this.$message.error(res)
          // })
          axios({
            // 请求方式
            method: 'post',
            // 后端接口路径
            url: urlStr,
            // 注意这里使用的是params,该属性负责把属性名和属性值添加到url后面，一般和get配合使用，但也能    和post配合使用
            params: {
              // attributeName为属性名，value为属性值
              value1: data.value1,
              value2: data.value2,
              appleQty: data.appleQty,
              strawberryQty: data.strawberryQty,
              mangoQty: data.mangoQty
            }
          }).then((response) => {
            debugger
            console.log(response)
            this.temp.sumPrice = response.data.items.totalPrice
            // eslint-disable-next-line handle-callback-err
          }).catch((error) => {
            // 对error进行处理
          })
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
