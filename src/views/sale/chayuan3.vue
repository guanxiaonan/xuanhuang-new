<template>
  <div class="app-container calendar-list-container">
    <el-card class="box-card">
      <h1>茶园3</h1>
      <el-table
        :data="tablechayuan"
        border
        style="width: 100%">
        <el-table-column
          fixed
          prop="caijidian"
          label="采集点"
          width="150">
        </el-table-column>
        <el-table-column
          fixed="right"
          label="操作"
          width="150"
        >
          <template slot-scope="scope">
            <!--<el-button @click="" type="text" size="small">实时数据</el-button>-->
            <el-button @click="selectHistory(scope.row)" type="text" size="small">历史数据</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
    <div id="main">
      <div id="left">
        <div :class="className" :id="id" :style="{height:height,width:width}" ref="myEchart"></div>
      </div>
      <div id="right">
        <img src="../../assets/401_images/1.png" alt="" style="width:150px;height:150px;">
        <img src="../../assets/401_images/1.png" alt="" style="width:150px;height:150px;">
        <img src="../../assets/401_images/1.png" alt="" style="width:150px;height:150px;">
        <img src="../../assets/401_images/1.png" alt="" style="width:150px;height:150px;">
        <img src="../../assets/401_images/1.png" alt="" style="width:150px;height:150px;">
        <img src="../../assets/401_images/1.png" alt="" style="width:150px;height:150px;">ß
        <el-button class="filter-item" style="margin-top:2%;margin-left:38%;" @click="viewMore">查看更多</el-button>
      </div>
    </div>
    <hr>
    <div class="filter-container">
      <el-button class="filter-item" style="float:left;" icon="el-icon-edit" @click="handleCreate">添加记录</el-button>
      <el-button style="float:right" class="filter-item" type="primary" v-waves icon="el-icon-search" @click="handleFilter">搜索</el-button>
      <el-input @keyup.enter.native="handleFilter" style="float:right; width:300px " class="filter-item" placeholder="检测时间/茶园编号/传感器编号"
                v-model="requestList.searchString">
      </el-input>
    </div>
    <el-table :key='tableKey' :data="list" v-loading="listLoading" element-loading-text="给我一点时间" border fit highlight-current-row
              style="width: 100%">
      <el-table-column align="center" type="index" :index="tIndex" label="序号" width="60">
      </el-table-column>
      <el-table-column min-width="150px" label="检测时间">
        <template slot-scope="scope">
          <img src="scope.row.productImg" border="1" alt="">
          <span style="float:right">{{scope.row.productName}}</span>
        </template>
      </el-table-column>
      <el-table-column width="110px" align="center" label="茶园编号">
        <template slot-scope="scope">
          <span>{{scope.row.shopName}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="传感器编号" width="95">
        <template slot-scope="scope">
          <span>{{scope.row.stock}}</span>
        </template>
      </el-table-column>
      <el-table-column align="center" label="数据" width="95">
        <template slot-scope="scope">
          <span>{{scope.row.saleAmount}}</span>
        </template>
      </el-table-column>
      <!-- <el-table-column class-name="status-col" label="数据" width="100">
        <template slot-scope="scope">
          <button type="primary" size="mini" @click="change(scope.row)">{{scope.row.status| statusFilter}}</button>
        </template>
      </el-table-column> -->
      <el-table-column align="center" :label="$t('table.actions')" width="230" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button type="primary" size="mini" @click="handleUpdate(scope.row)">编辑</el-button>
          <el-button size="mini" type="danger" @click="delete1(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <div class="pagination-container">
      <el-pagination background @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="request.page"
                     :page-sizes="[10,20,30,50]" :page-size="request.size" layout="total, sizes, prev, pager, next, jumper" :total="total">
      </el-pagination>
    </div>
    <el-dialog :title="textMap[dialogStatus]" :visible.sync="dialogfaVisible">
      <el-form ref="form" :model="form" :rules="rules" label-width="80px">
        <el-tabs v-model="activeName">
          <el-tab-pane label="土壤数据记录" name="first">
            <el-form-item label="检测时间">
              <span>&nbsp;</span>
              <el-col :span="6">
                <el-input style="margin-left:9px" v-model="form.price"></el-input>
              </el-col>
              <el-col style="margin-left:20px" :span="4">检测人姓名</el-col>
              <el-col :span="6">
                <el-input v-model="form.cost"></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="茶园编号">
              <span>&nbsp;</span>
              <el-col :span="6">
                <el-input style="margin-left:9px" v-model="form.price"></el-input>
              </el-col>
              <el-col style="margin-left:20px" :span="4">传感器编号</el-col>
              <el-col :span="6">
                <el-input v-model="form.cost"></el-input>
              </el-col>
            </el-form-item>
            <el-form-item label="土壤数据">
              <span>&nbsp;</span>
              <el-input style="width:150px" v-model="form.postFare"></el-input>
            </el-form-item>
          </el-tab-pane>
          <br>
          <el-button style="float:right" v-if="dialogStatus=='create'" type="primary" @click="submitForm('form')">确认</el-button>
          <el-button style="float:right" v-else type="primary" @click="submitForm1('form')">确认</el-button>
        </el-tabs>
      </el-form>
    </el-dialog>
    <!-- 选择查看数据类型-->

    <!--<el-button type="text" @click="history_5 = true">打开嵌套表单的 Dialog</el-button>-->
    <el-dialog title="数据查询" :visible.sync="history_5">
      <el-form :model="form_his">
        <!--<el-form-item label="活动名称" :label-width="formLabelWidth">-->
        <!--<el-input v-model="form.name" autocomplete="off"></el-input>-->
        <!--</el-form-item>-->
        <el-form-item label="查询数据" :label-width="formLabelWidth">
          <el-select v-model="form_his.type" placeholder="选择查询的数据">
            <el-option label="土壤温湿度" value="turang"></el-option>
            <el-option label="空气温湿度" value="kongqi"></el-option>
            <el-option label="光照强度" value="gaungzhao"></el-option>
            <el-option label="离子浓度" value="lizi"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="history_5 = false">取 消</el-button>
        <el-button type="primary" @click="history_Submit">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  import Dropzone from '@/components/Dropzone'
  import Tinymce from '@/components/Tinymce'
  import echarts from 'echarts'
  import {
    getProductInfo, // 组件-查看组件详情
    getProductList, // 组件-组件列表
    getSelectProductList,
    setProductInfo, // 组件保存组件
    getProductDetail, // 组件-统计信息
    setProductStatus, // 组件-设置方案状态
    getAllShopList // 组件方案 - 商家列表
  } from '@/api/article'
  import waves from '@/directive/waves' // 水波纹指令
  import { getToken } from '@/utils/auth'
  export default {
    props: {
      className: {
        type: String,
        default: 'yourClassName'
      },
      id: {
        type: String,
        default: 'yourID'
      },
      width: {
        type: String,
        default: '500px'
      },
      height: {
        type: String,
        default: '400px'
      }
    },
    name: 'complexTable',
    components: {
      Dropzone,
      Tinymce
    },
    directives: {
      waves
    },
    data() {
      return {
        tablechayuan: [
          {
            caijidian: '数据采集点1'
          },
          {
            caijidian: '数据采集点2'
          }
        ],
        caiji: '',
        biaoti: '',
        form_his: {
          type: ''
        },
        formLabelWidth: '120px',
        request: {
          authorization: '',
          productId: '',
          action: '',
          page: 1,
          size: 5
        },
        requestList: {
          authorization: '',
          type: 1,
          productId: '',
          page: 1,
          size: 5,
          searchString: '',
          status: 3
        },
        saleingCount: 0,
        stockCount: 0,
        saleoutCount: 0,
        form: {
          categoryId: '',
          productNo: '',
          id: '',
          name: '',
          shopId: '',
          shopName: '',
          price: '',
          detail: '',
          image: [],
          sortedNum: '',
          status: '',
          postFare: '',
          cost: '',
          primecost: '',
          stock: '',
          isvalid: '',
          fileList: [],
          shopIds: [],
          replaceProductVos: [{
            id: '',
            productId: '',
            productName: ''
          }]
          // newcomponentVos: []
        },
        form1: {
          content: ''
        },
        activeName: 'first',
        tableKey: 0,
        list: [],
        total: null,
        listLoading: true,
        dialogfaVisible: false,
        history_5: false,
        history_show: false,
        dialogStatus: '',
        textMap: {
          update: '编辑',
          create: '添加土壤数据'
        },
        rules: {
          name: [{
            required: true,
            message: 'name is required',
            trigger: 'change'
          }],
          image: [{

            required: true,
            message: 'image is required',
            trigger: 'change'
          }],
          stock: [{
            required: true,
            message: 'stock is required',
            trigger: 'change'
          }]
        },
        options: [],
        options1: [],
        downloadLoading: false,
        productIds: []
      }
    },
    filters: {
      statusFilter(type) {
        const typeMap = {

          '0': '下架',
          '1': '上架'
        }
        return typeMap[type]
      }
    },
    created() {
      this.getList()
      this.getNumber()
    },
    mounted() {
      // this.initChart()
      // this.history_Submit()
    },
    methods: {
      // init() {
      //   this.history_5 = false
      // },
      initChart() {
        this.chart = echarts.init(this.$refs.myEchart)
        // 把配置和数据放这里
        this.chart.setOption(
          {
            title: {
              text: this.biaoti,
              subtext: '纯属虚构'
            },
            tooltip: {
              trigger: 'axis'
            },
            legend: {
              data: ['标准温湿度', '实际温湿度']
            },
            toolbox: {
              show: true,
              feature: {
                dataZoom: {
                  yAxisIndex: 'none'
                },
                dataView: { readOnly: false },
                magicType: { type: ['line', 'bar'] },
                restore: {},
                saveAsImage: {}
              }
            },
            xAxis: {
              type: 'category',
              boundaryGap: false,
              data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
            },
            yAxis: {
              type: 'value',
              axisLabel: {
                formatter: '{value} °C'
              }
            },
            series: [
              {
                name: '最高气温',
                type: 'line',
                data: [11, 11, 15, 13, 12, 13, 10],
                markPoint: {
                  data: [
                    { type: 'max', name: '最大值' },
                    { type: 'min', name: '最小值' }
                  ]
                },
                markLine: {
                  data: [
                    { type: 'average', name: '平均值' }
                  ]
                }
              },
              {
                name: '最低气温',
                type: 'line',
                data: [1, -2, 2, 5, 3, 2, 0],
                markPoint: {
                  data: [
                    { name: '周最低', value: -2, xAxis: 1, yAxis: -1.5 }
                  ]
                },
                markLine: {
                  data: [
                    { type: 'average', name: '平均值' },
                    [{
                      symbol: 'none',
                      x: '90%',
                      yAxis: 'max'
                    }, {
                      symbol: 'circle',
                      label: {
                        normal: {
                          position: 'start',
                          formatter: '最大值'
                        }
                      },
                      type: 'max',
                      name: '最高点'
                    }]
                  ]
                }
              }
            ]
          }
        )
      },
      change(row) {
        this.request.authorization = getToken('Admin-Token')
        this.request.productId = row.productId
        this.$confirm('你确定将此方案下架吗, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          setProductStatus(this.request).then(response => {
            this.request.action = 1
            console.log('response', response)
            if (response.data.code ===
              '200') {
              this.getList()
              this.$message({
                type: 'success',
                message: '下架成功'
              })
            }
          })
        })
      },
      delete1(row) {
        this.request.authorization = getToken('Admin-Token')
        this.request.productId = row.productId
        this.$confirm('你确定将此方案删除吗, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          setProductStatus(this.request).then(response => {
            this.request.action = 0
            console.log('response', response)
            if (response.data.code ===
              '200') {
              this.getList()
              this.$message({
                type: 'success',
                message: '删除成功'
              })
            }
          })
        })
      },
      getNumber() {
        this.requestList.authorization = getToken('Admin-Token')
        getProductDetail(this.requestList.authorization).then(response => {
          console.log('number', response)
          this.saleoutCount = response.data.data.saleoutCount
        })
      },
      addItem() {
        this.form.productVos.push({
          productName: ''
        })
      },
      submitForm(form) {
        console.log('image', this.form.image)
        this.$refs.form.validate(valid => {
          if (valid && this.form.image) {
            setProductInfo(this.form).then(Response => {
              this.$message('修改成功')
              this.resetTemp()
              this.dialogfaVisible = false
            })
          } else {
            this.$message('还有未填选项')
            return false
          }
        })
      },
      submitForm1(form) {
        console.log('image', this.form.image)
        this.$refs.form.validate(valid => {
          if (valid && this.form.image) {
            // this.request.sampleIds = row.sampleIds
            setProductInfo(this.form).then(Response => {
              this.$message('修改成功')
              this.resetTemp()
              this.dialogfaVisible = false
            })
          } else {
            this.$message('还有未填选项')
            return false
          }
        })
      },
      handleRemove(file, image) {
        console.log('file', file)
        console.log('image', image)
      },
      changeImage(file) {
        this.form.image.push(file)
      },
      handleSuccess(response, file, fileList) {
        this.form.image = response.data.imageUrl
      },
      handlePreview(file) {
        console.log(file)
      },
      tIndex(index) {
        return (index + 1 + (this.request.page - 1) * this.request.size)
      },
      onSubmit() {
        console.log('submit!')
      },
      getList() {
        this.listLoading = true
        this.requestList.authorization = getToken('Admin-Token')
        getProductList(this.requestList).then(response => {
          console.log('list', response)
          this.list = response.data.data.productList
          this.total = response.data.data.total
          this.listLoading = false
        })
        getSelectProductList(this.requestList.authorization).then(res => {
          this.options = res.data.data
        })
        getAllShopList(this.requestList.authorization).then(res => {
          this.options1 = res.data.data
        })
      },
      beforeAvatarUpload(file) {
        console.log('length', file)
        const isJPG = file.type === 'image/jpeg/png/jpg'
        // const isLt2M = file.size / 1024 / 1024 < 2
        if (!isJPG) {
          this.$message.error('上传图片只能是 JPG/png/jpeg 格式!')
        }
        // if (!isLt2M) {
        //   this.$message.error('上传图片大小不能超过 2MB!')
        // }
        return isJPG
        // && isLt2M
      },
      handleFilter() {
        this.request.page = 1
        this.getList()
      },
      handleSizeChange(val) {
        this.request.size = val
        this.getList()
      },
      handleCurrentChange(val) {
        this.request.page = val
        this.getList()
      },
      handleModifyStatus(row, status) {
        this.$message({
          message: '操作成功',
          type: 'success'
        })
        row.status = status
      },
      resetTemp() {
        this.form = {
          categoryId: '',
          productNo: '',
          id: '',
          name: '',
          shopId: '',
          shopName: '',
          price: '',
          detail: '',
          image: [],
          sortedNum: '',
          status: '',
          postFare: '',
          cost: '',
          primecost: '',
          stock: '',
          isvalid: '',
          productVos: [{
            productIds: ''
          }],
          replaceProductVos: [{
            productName: ''
          }]
        }
      },
      selectHistory(row) {
        console.log('点击的行数', row)
        if (row.caijidian === '数据采集点1') {
          this.caiji = '采集点1'
        } else if (row.caijidian === '数据采集点2') {
          this.caiji = '采集点2'
        }
        this.history_5 = true
        // this.history_show = true
      },
      history_Submit() {
        if (this.form_his.type === 'turang') {
          this.biaoti = '茶园3' + this.caiji + '-土壤温湿度表'
        } else if (this.form_his.type === 'kongqi') {
          this.biaoti = '茶园3' + this.caiji + '-空气温湿度表'
        } else if (this.form_his.type === 'gaungzhao') {
          this.biaoti = '茶园3' + this.caiji + '-光照度表'
        } else if (this.form_his.type === 'lizi') {
          this.biaoti = '茶园3' + this.caiji + '-离子浓度表'
        }
        this.initChart()
        this.history_5 = false
        // alert(this.form_his.type)
        this.history_show = true
        console.log('history_show:', this.history_show)
        let div1 = document.getElementById('right');
        div1.style.visibility='visible';
      },
      handleCreate() {
        this.resetTemp()
        this.dialogStatus = 'create'
        this.dialogfaVisible = true
        this.productIds = []
        this.shopIds = []
        this.$nextTick(() => {
          this.$refs['form'].clearValidate()
        })
      },
      handleUpdate(row) {
        this.dialogStatus = 'update'
        this.dialogfaVisible = true
        this.$nextTick(() => {
          this.$refs['form'] && this.$refs['form'].clearValidate()
        })
        this.requestList.productId = row.productId
        this.requestList.authorization = getToken('Admin-Token')
        getProductInfo(this.requestList).then(response => {
          console.log('111', response)
          this.form = response.data.data
          console.log(this.form)
          const productIds = this.filterId(this.form.replaceProductVos)
          this.form.productVos = [{
            productIds
          }]
          // this.form.forEach(item => {
          //   this.shopIds = this.filterId1(item.shopName)
          // })

          // console.log('responseaaaaa', response)
        })
      },
      filterId(objArr) {
        const arr = []
        if (objArr && objArr.length > 0) {
          objArr.forEach((obj) => {
            arr.push(obj.productId + '')
          })
        }
        return arr
      },
      filterId1(objArr) {
        const arr = []
        if (objArr && objArr.length) {
          objArr.forEach((obj) => {
            arr.push(obj.shopId + '')
          })
        }
        return arr
      },
      viewMore() {
        this.$router.push({
          name: 'image'
        })
      }
    },
    watch: {
      history_show(val) {
        console.log(val)
        this.history_show = val
      }
    }
  }
</script>
<style lang="scss" scoped>
  .el-dialog {
    .el-input {
      width: 98%;
    }
  }
</style>

<style>
  .element {
    height: 40px;
    line-height: 40px;
    margin-bottom: 12px;
  }
  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }
  .clearfix:after {
    clear: both
  }

  .box-card {
    width: 480px;
  }
  #main{
    height: 500px;
    margin-top:30px;
  }
  #left{
    position: absolute;
    width: 500px;
    height:400px;
  }
  #right{
    margin-left: 520px;
    visibility: hidden;
    height:400px;
  }
</style>
