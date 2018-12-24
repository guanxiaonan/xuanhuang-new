<template>
  <el-row class="panel-group" :gutter="80">
    <el-col :xs="24" :sm="24" :lg="6" class="card-panel-col">
      <div @click="chuli1" class='card-panel'>
        <div class="card-panel-description">
          <div class="card-panel-text">茶园1</div>
          <!--<span>{{data.saleoutproducts}}</span>-->
        </div>
      </div>
      <div>
        <el-collapse v-model="activeNames" @change="">
          <el-collapse-item title="实时采集点" name="1">
            <div>
              <el-popover
                placement="right"
                width="400"
                trigger="click">
                <el-table :data="gridData1">
                  <el-table-column width="150" property="date" label="日期"></el-table-column>
                  <el-table-column width="100" property="name" label="姓名"></el-table-column>
                  <el-table-column width="300" property="address" label="地址"></el-table-column>
                </el-table>
                <el-button slot="reference">采集点5实时数据</el-button>
              </el-popover>
            </div>
            <div>
            <el-popover
              placement="right"
              width="400"
              trigger="click">
              <el-table :data="gridData1">
                <el-table-column width="150" property="date" label="日期"></el-table-column>
                <el-table-column width="100" property="name" label="姓名"></el-table-column>
                <el-table-column width="300" property="address" label="地址"></el-table-column>
              </el-table>
              <el-button slot="reference">采集点6实时数据</el-button>
            </el-popover>
            </div>
            <div>
            <el-popover
              placement="right"
              width="400"
              trigger="click">
              <el-table :data="gridData1">
                <el-table-column width="150" property="date" label="日期"></el-table-column>
                <el-table-column width="100" property="name" label="姓名"></el-table-column>
                <el-table-column width="300" property="address" label="地址"></el-table-column>
              </el-table>
              <el-button slot="reference">采集点7实时数据</el-button>
            </el-popover>
            </div>
          </el-collapse-item>
        </el-collapse>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div @click="chuli2" class="card-panel">
        <div class="card-panel-description">
          <div class="card-panel-text">茶园2</div>
          <!--<span>{{data.deliverySO}}</span>-->
        </div>
      </div>
      <div>
        <el-collapse v-model="activeNames" @change="">
          <el-collapse-item title="实时采集点" name="1">
            <div>
              <el-popover
                placement="right"
                width="400"
                trigger="click">
                <el-table :data="gridData1">
                  <el-table-column width="150" property="date" label="日期"></el-table-column>
                  <el-table-column width="100" property="name" label="姓名"></el-table-column>
                  <el-table-column width="300" property="address" label="地址"></el-table-column>
                </el-table>
                <el-button slot="reference">采集点3实时数据</el-button>
              </el-popover>
            </div>
            <div>
              <el-popover
                placement="right"
                width="400"
                trigger="click">
                <el-table :data="gridData1">
                  <el-table-column width="150" property="date" label="日期"></el-table-column>
                  <el-table-column width="100" property="name" label="姓名"></el-table-column>
                  <el-table-column width="300" property="address" label="地址"></el-table-column>
                </el-table>
                <el-button slot="reference">采集点4实时数据</el-button>
              </el-popover>
            </div>
          </el-collapse-item>
        </el-collapse>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div @click="chuli3" class="card-panel">
        <div class="card-panel-description">
          <div class="card-panel-text">茶园3</div>
          <!--<span>{{data.questionSO}}</span>-->
        </div>
      </div>
      <div>
        <el-collapse v-model="activeNames" @change="">
          <el-collapse-item title="实时采集点" name="1">
            <div>
              <el-popover
                placement="right"
                width="400"
                trigger="click">
                <el-table :data="gridData1">
                  <el-table-column width="150" property="date" label="日期"></el-table-column>
                  <el-table-column width="100" property="name" label="姓名"></el-table-column>
                  <el-table-column width="300" property="address" label="地址"></el-table-column>
                </el-table>
                <el-button slot="reference">采集点1实时数据</el-button>
              </el-popover>
            </div>
            <div>
              <el-popover
                placement="right"
                width="400"
                trigger="click">
                <el-table :data="gridData1">
                  <el-table-column width="150" property="date" label="日期"></el-table-column>
                  <el-table-column width="100" property="name" label="姓名"></el-table-column>
                  <el-table-column width="300" property="address" label="地址"></el-table-column>
                </el-table>
                <el-button slot="reference">采集点2实时数据</el-button>
              </el-popover>
            </div>
          </el-collapse-item>
        </el-collapse>
      </div>
    </el-col>
  </el-row>
</template>

<script>
  export default {
    components: {},
    props: {
      data: {
        saleoutproducts: {
          type: Number,
          default: 0
        },
        deliverySO: {
          type: Number,
          default: 0
        },
        questionSO: {
          type: Number,
          default: 0
        },
        shenheCash: {
          type: Number,
          default: 0
        }
      }

    },
    data() {
      return {
        gridData1: [{
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
      }
    },
    methods: {
      chuli1() {
        this.$router.push({
          name: 'chayuan1'
        })
        // alert('chuli1')
      },
      chuli2() {
        // this.$router.push({
        //   name: 'home_index'
        // })
        alert('chuli2')
      },
      chuli3() {
        // this.$router.push({
        //   name: 'home_index'
        // })
        alert('chuli3')
      }
    }
  }

</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .card {
    margin-left: 100px;
  }


   .el-card__body {
    padding: 5px;
  }

  .panel-group {
    margin-top: 18px;
    text-align: center;

    .card-panel-col {
      margin-bottom: 32px;
    }

    .card-panel {
      text-align: center;
      height: 80px;
      cursor: pointer;
      font-size: 12px;
      position: relative;
      overflow: hidden;
      color: #666;
      background: #fff;
      box-shadow: 4px 4px 40px rgba(0, 0, 0, .05);
      border-color: rgba(0, 0, 0, .05); // &:hover {


      //   .card-panel-icon-wrapper {
      //     color: #fff;
      //   }
      //   .icon-people {
      //      background: #40c9c6;
      //   }
      //   .icon-message {
      //     background: #36a3f7;
      //   }
      //   .icon-money {
      //     background: #f4516c;
      //   }
      //   .icon-shoppingCard {
      //     background: #34bfa3
      //   }
      // }
      // .icon-people {
      //   color: #40c9c6;
      // }
      // .icon-message {
      //   color: #36a3f7;
      // }
      // .icon-money {
      //   color: #f4516c;
      // }
      // .icon-shoppingCard {
      //   color: #34bfa3
      // }
      // .card-panel-icon-wrapper {
      //   float: left;
      //   margin: 14px 0 0 14px;
      //   padding: 16px;
      //   transition: all 0.38s ease-out;
      //   border-radius: 6px;
      // }
      // .card-panel-icon {
      //   float: left;
      //   font-size: 48px;
      // }
      .card-panel-description {
        position: absolute;
        left: 50%;
        top: 50%;
        width: 100px;
        height: 60px;
        font-weight: bold;
        margin-left: -50px;
        margin-top: -15px;

        .card-panel-text {
          line-height: 18px;
          color: rgba(0, 0, 0, 0.45);
          font-size: 16px;
          margin-bottom: 12px;
        }

        .card-panel-num {
          font-size: 20px;
        }
      }
    }
  }

</style>
<style>
.el-card__body{
  padding:5px;
}
</style>

