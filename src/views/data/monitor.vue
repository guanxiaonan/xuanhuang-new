<template>
    <div :class="className" :id="id" :style="{height:height,width:width}" ref="myEchart">
    </div>
  <!--<div class="box">-->
    <!--<div class="top">-->
      <!--<el-tooltip class="item" effect="dark" content="Top Left 提示文字" placement="top-start">-->
        <!--<el-button>上左</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Top Center 提示文字" placement="top">-->
        <!--<el-button>上边</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Top Right 提示文字" placement="top-end">-->
        <!--<el-button>上右</el-button>-->
      <!--</el-tooltip>-->
    <!--</div>-->
    <!--<div class="left">-->
      <!--<el-tooltip class="item" effect="dark" content="Left Top 提示文字" placement="left-start">-->
        <!--<el-button>左上</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Left Center 提示文字" placement="left">-->
        <!--<el-button>左边</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Left Bottom 提示文字" placement="left-end">-->
        <!--<el-button>左下</el-button>-->
      <!--</el-tooltip>-->
    <!--</div>-->

    <!--<div class="right">-->
      <!--<el-tooltip class="item" effect="dark" content="Right Top 提示文字" placement="right-start">-->
        <!--<el-button>右上</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Right Center 提示文字" placement="right">-->
        <!--<el-button>右边</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Right Bottom 提示文字" placement="right-end">-->
        <!--<el-button>右下</el-button>-->
      <!--</el-tooltip>-->
    <!--</div>-->
    <!--<div class="bottom">-->
      <!--<el-tooltip class="item" effect="dark" content="Bottom Left 提示文字" placement="bottom-start">-->
        <!--<el-button>下左</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Bottom Center 提示文字" placement="bottom">-->
        <!--<el-button>下边</el-button>-->
      <!--</el-tooltip>-->
      <!--<el-tooltip class="item" effect="dark" content="Bottom Right 提示文字" placement="bottom-end">-->
        <!--<el-button>下右</el-button>-->
      <!--</el-tooltip>-->
    <!--</div>-->
  <!--</div>-->
  <!--<el-tooltip content="Bottom center" placement="bottom" effect="light">-->
    <!--<el-button>Light</el-button>-->
  <!--</el-tooltip>-->
</template>
<script>
    import echarts from 'echarts'
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
          default: '1000px'
        },
        height: {
          type: String,
          default: '500px'
        }
      },
      data() {
        return {
          data1: [5, 6, 2, 3, 4, 1, 2, 3, 5, 7, 8, 90, 6, 4, 4, 2, 2, 3, 6, 6, 86, 87, 6, 5, 43, 23, 342],
          data2: [2016 - 1 - 2, 2016 - 1 - 3, 2016 - 1 - 4, 2016 - 1 - 5, 2016 - 1 - 2, 2016 - 1 - 3, 2016 - 1 - 4, 2016 - 1 - 5, 2016 - 1 - 2, 2016 - 1 - 3, 2016 - 1 - 4, 2016 - 1 - 5],
          chart: null
        }
      },
      mounted() {
        this.initChart()
      },
      beforeDestroy() {
        if (!this.chart) {
          return
        }
        this.chart.dispose()
        this.chart = null
      },
      methods: {
        initChart() {
          this.chart = echarts.init(this.$refs.myEchart)
          // 把配置和数据放这里
          this.chart.setOption({
            title: {
              text: '黄茶EC'
            },
            tooltip: {
              trigger: 'axis'
            },
            xAxis: {
              data: this.data2
            },
            yAxis: {
              splitLine: {
                show: false
              }
            },
            toolbox: {
              left: 'center',
              feature: {
                dataZoom: {
                  yAxisIndex: 'none'
                },
                restore: {},
                saveAsImage: {}
              }
            },
            dataZoom: [{
              startValue: '2014-06-01'
            }, {
              type: 'inside'
            }],
            visualMap: {
              top: 10,
              right: 10,
              pieces: [{
                gt: 0,
                lte: 50,
                color: '#096'
              }, {
                gt: 50,
                lte: 100,
                color: '#ffde33'
              }, {
                gt: 100,
                lte: 150,
                color: '#ff9933'
              }, {
                gt: 150,
                lte: 200,
                color: '#cc0033'
              }, {
                gt: 200,
                lte: 300,
                color: '#660099'
              }, {
                gt: 300,
                color: '#7e0023'
              }],
              outOfRange: {
                color: '#999'
              }
            },
            series: {
              name: 'Beijing AQI',
              type: 'line',
              data: this.data1,
              markLine: {
                silent: true,
                data: [{
                  yAxis: 50
                }, {
                  yAxis: 100
                }, {
                  yAxis: 150
                }, {
                  yAxis: 200
                }, {
                  yAxis: 300
                }]
              }
            }
          })
        }
      }
    }
</script>

<style>
  .box {
    width: 400px;

    .top {
      text-align: center;
    }

    .left {
      float: left;
      width: 60px;
    }

    .right {
      float: right;
      width: 60px;
    }

    .bottom {
      clear: both;
      text-align: center;
    }

    .item {
      margin: 4px;
    }

    .left .el-tooltip__popper,
    .right .el-tooltip__popper {
      padding: 8px 10px;
    }
  }
</style>
