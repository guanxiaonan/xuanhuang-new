<template>
    <div :class="className" :id="id" :style="{height:height,width:width}" ref="myEchart">
    </div>
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
                data1:[5,6,2,3,4,1,2,3,5,7,8,90,6,4,4,2,2,3,6,6,86,87,6,5,43,23,342,],
                data2:[2016-1-2,2016-1-3,2016-1-4,2016-1-5,2016-1-2,2016-1-3,2016-1-4,2016-1-5,2016-1-2,2016-1-3,2016-1-4,2016-1-5],
                chart: null
            }
        },
        mounted() {
            this.initChart();
        },
        beforeDestroy() {
            if (!this.chart) {
                return
            }
            this.chart.dispose();
            this.chart = null;
        },
        methods: {
            initChart() {
                this.chart = echarts.init(this.$refs.myEchart);
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
