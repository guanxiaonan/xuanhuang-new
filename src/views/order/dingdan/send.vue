<template>
  <div class="app-container calendar-list-container">
    <h4>待发货订单</h4>
    <div class="filter-container">
      <el-select style="width: 120px" v-model="request.isSearchAsTime" placeholder="不按时间">
        <el-option v-for="item in TypeOptions" :key="item.key" :label="item.display_name" :value="item.key">
        </el-option>
      </el-select>
      <el-date-picker v-model="searchT" type="datetimerange" @change="changeDate" range-separator="至" start-placeholder="开始日期"
        end-placeholder="结束日期">
      </el-date-picker>
      <!-- <el-select style="width: 120px" v-model="request.status" placeholder="选择状态">
        <el-option v-for="item in  calendarTypeOptions" :key="item.key" :label="item.display_name" :value="item.key">
        </el-option>
      </el-select> -->
      <el-button style="float:right" v-waves icon="el-icon-search" @click="handleFilter">搜索</el-button>
      <el-input @keyup.enter.native="handleFilter" style="width: 150px;float:right" placeholder="请输入关键词" v-model="request.searchString">
      </el-input>
    </div>
    <table class="tabletitle" cellpadding="0" cellspacing="0">
      <tr style="width:100%;height:44px">
        <th style="width:26%" class="border-right">商品</th>
        <th style="width:8%" class="border-right">单价/数量</th>
        <th style="width:14%" class="border-right">买家</th>
        <th style="width:13%" class="border-right">支付/配送</th>
        <th style="width:13%" class="border-right">价格</th>
        <th style="width:13%" class="border-right">下单时间</th>
        <th style="width:13%">状态</th>
      </tr>
    </table>
    <table class="tabletitle2" cellpadding="0" cellspacing="0">
      <tbody style="width:100%" v-for="(table,index) of tablelist" :key="index">
        <tr>
          <th style="width:26%;text-align:left;font-size:14px" class="border-bottom">订单编号：{{table.orderNo}}</th>
          <th style="width:8%;font-size:14px" class="border-bottom">退款申请</th>
          <th style="width:14%" class="border-bottom"></th>
          <th style="width:13%" class="border-bottom"></th>
          <th style="width:13%" class="border-bottom"></th>
          <th colspan="2" class="border-bottom">
            <button style="font-size:10px" type="mini">删除订单</button>
            <router-link :to="{path:'/order/rights/detial',query:{orderNo:table.orderNo}}">
              <button v-if="table.returnStatus" style="font-size:10px" type="mini">维权处理</button>
            </router-link>
            <router-link :to="{path:'/order/dingdan/more',query:{SOId:table.orderNo}}">
              <button style="font-size:10px">查看详情</button>
            </router-link>
            <button v-if="table.note" style="font-size:10px" type="mini" @click="submit2(table)">备注</button>
          </th>
        </tr>
        <tr style="font-size:14px;">
          <td style="width:26%">
            <div style="clear:both;">
              <div class="commonstyle">
                这是图片
              </div>
              <div class="commonstyle">
                <p>{{table.note}}</p>
              </div>
            </div>
          </td>
          <td>
            <div style="text-align:center;" class="border-right2">
              <p>{{table.price}}</p>
              <p>*{{table.amount}}</p>
            </div>
          </td>
          <td>
            <div style="text-align:center;" class="border-right2">
              <p>{{table.orderInfo}}</p>
              <p>{{table.userPhone}}</p>
            </div>
          </td>
          <td>
            <div style="text-align:center;" class="border-right2">
              <p>{{table.payType}}</p>
              <p>{{table.deliveryType}}</p>
            </div>
          <td>
            <div style="text-align:center;" class="border-right2">
              <p>¥{{parseFloat(table.price) + parseFloat(table.postFare)}}</p>
              <p>(含运费：¥{{table.postFare}})</p>
            </div>
          </td>
          <td>
            <div style="text-align:center;" class="border-right2">
              <p>{{table.createTime|parseTime('{yyyy}-{mm}-{dd} {hh}:{ii}:{ss}')}}</p>
            </div>
          </td>
          <td>
            <div style="text-align:center;" class="border-right2" vvv>
              <p>待发货</p>
              <button class="filter-item" @click="checkSend(table)">确认发货</button>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <div class="pagination-container">
      <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="request.page"
        :page-sizes="[10,20,30, 50]" :page-size="request.size" layout="total, sizes, prev, pager, next, jumper" :total="total">
      </el-pagination>
    </div>
    <el-dialog title="确认发货" :visible.sync="dialogfhVisible">

      <el-form ref="ruleForm" :rules="rules" :model="ruleForm" label-width="200px">


        <el-form-item class="label" label="收货地址">

          <p style="margin-top:0px">{{ruleForm.receiveAddressInfo}}</p>


        </el-form-item>

        <el-form-item style="margin-top:0px" class="label" label="快递公司" prop="deliveryCompany">
          <el-input style="width:80%;float:left" v-model="ruleForm.deliveryCompany">

          </el-input>
        </el-form-item>
        <el-form-item class="label" label="快递单号" prop="deliveryNo">
          <el-input style="width:80%;float:left" v-model="ruleForm.deliveryNo"></el-input>
        </el-form-item>
      </el-form>

      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submit(ruleForm)">确认发货</el-button>
      </span>
    </el-dialog>
    <!-- <el-dialog title="备注" :visible.sync="beizhu">
      <hr>
      <textarea v-model="res.remark" name="txt" id="txt" cols="30" rows="10" style="width:100%"></textarea>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submit1()">提交</el-button>
        <el-button type="primary" @click="beizhu = false">取消</el-button>
      </span>
    </el-dialog> -->
    <el-dialog title="备注" :visible.sync="beizhu">
      <hr>

      <textarea style="width:100%" name="txt" id="txt" cols="30" rows="10" v-model="form.note"></textarea>

      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submit1('form')">提交</el-button>
        <el-button type="primary" @click="beizhu = false">取消</el-button>
      </span>
    </el-dialog>
    <el-dialog title="备注" :visible.sync="beizhu1">
      <hr>
      <textarea style="width:100%" name="txt" id="txt" cols="30" rows="10" v-model="table.note"></textarea>
    </el-dialog>
  </div>
</template>

<script>
  import {
    getSODetail,
    modifySaleOrderRemark,
    searchSaleOrders,
    comfirmShipment
  } from '@/api/article'
  import waves from '@/directive/waves' // 水波纹指令
  import {
    getToken
  } from '@/utils/auth'
  const calendarTypeOptions = [{
    key: 0,
    display_name: '未付款'
  },
  {
    key: 1,
    display_name: '待发货'
  },
  {
    key: 2,
    display_name: '待收货'
  },
  {
    key: 3,
    display_name: '已完成'
  },
  {
    key: 4,
    display_name: '已关闭'
  }
  ]
  const TypeOptions = [{
    key: 0,
    display_name: '不按时间'
  },
  {
    key: 1,
    display_name: '按时间'
  }

  ]

  export default {
    name: 'pay',
    directives: {
      waves
    },

    data() {
      return {
        requset: {
          SOId: ''
        },
        form: {
          orderNo: '',
          note: ''
        },
        searchT: [],
        res: {
          orderNo: '',
          remark: ''
        },
        request: {
          authorization: '',
          page: 1,
          size: 10,
          paymode: '',
          startTime: '',
          endTime: '',
          isSearchAsTime: 0,
          status: 1
        },
        tablelist: [],
        table: {
          note: '',
          orderNo: '',
          price: '',
          postFare: '',
          amount: '',
          orderInfo: '',
          userPhone: '',
          payType: '',
          deliveryType: '',
          createTime: ''
        },
        tableKey: 0,
        list: null,
        total: null,
        listLoading: true,
        listQuery: {
          page: 1,
          limit: 20,
          importance: undefined,
          title: undefined,
          type: undefined,
          sort: '+id'
        },
        importanceOptions: [1, 2, 3],
        calendarTypeOptions,
        TypeOptions,
        beizhu: false,
        beizhu1: false,
        sortOptions: [{
          label: 'ID Ascending',
          key: '+id'
        }, {
          label: 'ID Descending',
          key: '-id'
        }],
        statusOptions: ['published', 'draft', 'deleted'],
        showReviewer: false,
        temp: {
          id: undefined,
          importance: 1,
          remark: '',
          timestamp: new Date(),
          title: '',
          type: '',
          status: 'published'
        },
        dialogfhVisible: false,
        dialogFormVisible: false,
        dialogStatus: '',
        textMap: {
          update: 'Edit',
          create: 'Create'
        },
        dialogPvVisible: false,
        pvData: [],
        ruleForm: {
          authorization: '',
          deliveryCompany: '',
          deliveryNo: '',
          orderNo: '',
          receiveAddressInfo: ''
        },
        rules: {
          deliveryCompany: [{
            required: true,
            message: '请输入快递公司',
            trigger: 'change'
          }],
          deliveryNo: [{
            required: true,
            message: '请输入快递单号',
            trigger: 'blur'
          }]
        },
        downloadLoading: false
      }
    },
    created() {
      this.getList()
    },
    methods: {
      getList() {
        this.listLoading = true
        this.request.authorization = getToken('Admin-Token')
        searchSaleOrders(this.request).then(response => {
          this.tablelist = response.data.data
          console.log('response', response)
          this.listLoading = false
        })
      },
      getDetial() {
        this.listLoading = true
        getSODetail(this.requset).then(response => {
          this.list = response.data.data.products
          this.form.note = response.data.data.note
          this.ruleForm.receiveAddressInfo = response.data.data.receiveAddressInfo
          this.listLoading = false
        })
      },
      submit2(table) {
        this.beizhu1 = true
        this.request.authorization = getToken('Admin-Token')
        searchSaleOrders(this.request).then(response => {
          this.table.note = response.data.data.note
          console.log('response', response)
          this.listLoading = false
        })
      },
      submit1(form) {
        this.beizhu = false
        this.res.orderNo = this.detailData.orderNo
        this.res.remark = this.form.note
        modifySaleOrderRemark(this.res).then(Response => {
          this.$message('提交成功')
        })
      },
      submit(ruleForm) {
        this.ruleForm.authorization = getToken('Admin-Token')
        // console.log(ruleForm)
        // this.ruleForm.orderNo = this.tablelist.orderNo
        this.$refs.ruleForm.validate(valid => {
          if (valid) {
            comfirmShipment(this.ruleForm).then(Response => {
              this.$message('修改成功')
              this.dialogfhVisible = false
            })
          } else {
            this.$message('还有未填选项')
            return false
          }
        })
      },
      checkSend(item) {
        this.dialogfhVisible = true
        this.ruleForm.orderNo = item.orderNo
      },
      handleFilter() {
        this.request.page = 1
        this.getList()
      },
      changeDate(val) {
        this.request.startTime = val[0]
        this.request.endTime = val[1]
      },
      handleSizeChange(val) {
        this.request.size = val
        this.getList()
      },
      handleCurrentChange(val) {
        this.request.page = val
        this.getList()
      }
    }
  }
</script>

<style lang="scss" scoped>
  .commonstyle {
    float: left;
  }

  .tabletitle {
    width: 100%;
    font-weight: bold;
    font-size: 14px;
    height: 44px;
    border: 1px solid #ebeef5;
    color: #909399;

    .border-right {
      border-right: 1px solid #ebeef5;
      font-weight: inherit;

    }
  }

  .tabletitle2 {
    width: 100%;
    font-weight: inherit;
    border: 1px solid #ebeef5;
    color: #606266;
    margin-top: 10px;
    font-weight: inherit;

    .border-bottom {
      border-bottom: 1px solid #ebeef5;
      font-weight: inherit;
    }

    .border-right2 {
      border-right: 1px solid #ebeef5;
      font-weight: inherit;
    }
  }
</style>
