<template>
  <el-row class="panel-group" :gutter="40">
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class='card-panel'>

        <div class="card-panel-description">
          <div class="card-panel-text">已售罄商品</div>
          <span>{{data.saleoutproducts}}</span>
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel">
        <div class="card-panel-description">
          <div class="card-panel-text">待发货订单</div>
          <span>{{data.deliverySO}}</span>
        </div>
      </div>
    </el-col>
    <el-col :xs="12" :sm="12" :lg="6" class="card-panel-col">
      <div class="card-panel">
        <div class="card-panel-description">
          <div class="card-panel-text">维权中订单</div>
          <span>{{data.questionSO}}</span>
        </div>
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

    methods: {}
  }
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .panel-group {
    margin-top: 18px;
    text-align: center;
    .card-panel-col {
      margin-bottom: 32px;
    }
    .card-panel {
      text-align: center;
      height: 108px;
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
        margin-top: -30px;
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
