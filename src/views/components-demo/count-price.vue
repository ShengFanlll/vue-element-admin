<template>
  <div>
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
        <el-input v-model="temp.appleQty" />
        <span style="font-size: 8px">8元每斤</span>
      </el-form-item>
      <el-form-item label="草莓" prop="strawberryQty">
        <el-input v-model="temp.strawberryQty" />
        <span style="font-size: 8px">13元每斤</span>
      </el-form-item>
      <el-form-item label="芒果" prop="mangoQty">
        <el-input v-model="temp.mangoQty" />
        <span style="font-size: 8px">20元每斤</span>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm(temp)">计算总价</el-button>
      </el-form-item>
      <el-form-item label="总价" prop="sumPrice">
        <el-input v-model="temp.sumPrice" :disabled="true" />
      </el-form-item>
    </el-form>

    <el-dialog title="账单" :visible.sync="billDialogVisible" width="60%" center>
      <div style="text-align: center">
        <el-table :data="tableData" style="width: 100%">
          <el-table-column type="index" min-width="2%" />
          <el-table-column label="商品" align="center" min-width="48%">
            <template slot-scope="scope">
              <p> {{ scope.row.commodityName }}</p>
            </template>
          </el-table-column>
          <el-table-column label="数量" align="center" min-width="25%">
            <template slot-scope="scope">
              <p> {{ scope.row.quality }}</p>
            </template>
          </el-table-column>
          <el-table-column label="价格" align="center" min-width="25%">
            <template slot-scope="scope">
              <p> {{ scope.row.itemPrice }}</p>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <span style="display: block; margin-left: 500px; margin-top: 25px; font-size: large">
        总价:
        <el-input v-model="temp.sumPrice" :disabled="true" style="border: none; width: 100px" />
      </span>
      <span slot="footer" class="dialog-footer">
<!--        <el-button @click="billDialogVisible = false">取 消</el-button>-->
        <el-button type="primary" @click="billDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
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
      tableData: [],
      billDialogVisible: false,
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
            // 注意这里使用的是params,该属性负责把属性名和属性值添加到url后面，一般和get配合使用，但也能和post配合使用
            params: {
              // attributeName为属性名，value为属性值
              value1: data.value1,
              value2: data.value2,
              appleQty: data.appleQty,
              strawberryQty: data.strawberryQty,
              mangoQty: data.mangoQty
            }
          }).then((response) => {
            if (response.status === 200) {
              this.$message({
                message: '成功计算!',
                type: 'success'
              })
              this.temp.sumPrice = response.data.items.totalPrice
              this.billDialogVisible = true
              debugger
              this.tableData = response.data.billDetailList
            } else {
              this.$message({
                message: '无数据!',
                type: 'warning'
              })
            }
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
