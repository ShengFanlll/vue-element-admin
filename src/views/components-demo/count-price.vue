<template>
  <el-form ref="dataForm" :rules="rules" :model="temp" label-position="left" label-width="100px" style="width: 400px; margin-left:50px;">

    <el-form-item label="开启活动1">
      <el-switch
        v-model="temp.value1"
      >
      </el-switch>
      <span style="color: red; font-size: 8px; margin-left: 10px">*草莓限时打8折</span>
    </el-form-item>
    <el-form-item label="开启活动2">
      <el-switch
        v-model="temp.value2"
      >
      </el-switch>
      <span style="color: red; font-size: 8px; margin-left: 10px">*购物满100减10快</span>
    </el-form-item>

    <el-form-item label="苹果" prop="applePrice">
      <el-input v-model="temp.applePrice"></el-input>
      <span style="font-size: 8px">8元每斤</span>
    </el-form-item>
    <el-form-item label="草莓" prop="strawberryPrice">
      <el-input v-model="temp.strawberryPrice"></el-input>
      <span style="font-size: 8px">13元每斤</span>
    </el-form-item>
    <el-form-item label="芒果" prop="mangoPrice">
      <el-input v-model="temp.mangoPrice"></el-input>
      <span style="font-size: 8px">20元每斤</span>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm('temp')">计算总价</el-button>
    </el-form-item>
    <el-form-item label="总价" prop="sumPrice">
      <el-input v-model="temp.sumPrice"></el-input>
    </el-form-item>
  </el-form>
</template>

<script>

export default {
  name: 'CountPriceDemo',
  components: {},
  data() {
    return {
      temp: {
        value1: false,
        value2: false,
        applePrice: '0',
        strawberryPrice: '0',
        mangoPrice: '0',
        sumPrice: '0'
      },
      rules: {
        applePrice: [{ pattern: /^[+]?(0|([1-9]\d*))?$/, message: '请输入大于等于0的整数', trigger: 'change' }],
        strawberryPrice: [{ pattern: /^[+]?(0|([1-9]\d*))?$/, message: '请输入大于等于0的整数', trigger: 'change' }],
        mangoPrice: [{ pattern: /^[+]?(0|([1-9]\d*))?$/, message: '请输入大于等于0的整数', trigger: 'change' }]
      }
    }
  },
  methods: {
    submitForm() {
      return null
    }
  }
}
</script>

<style scoped>

</style>
