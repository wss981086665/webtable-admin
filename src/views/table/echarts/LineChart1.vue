<template>
  <div :class="className" :style="{height:height,width:width}" />
</template>
<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme
import resize from './mixins/resize'

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '400px'
    },
    autoResize: {
      type: Boolean,
      default: true
    },
    chartData: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      chart: null
    }
  },
  watch: {
    chartData: {
      deep: true,
      handler(val) {
        this.setOptions(val)
      }
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart()
    })
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
      this.chart = echarts.init(this.$el, 'macarons')
      this.setOptions(this.chartData)
    },
    setOptions({ xname,yname,title, name, xdata, ydata } = {}) {
      var right = 0;
      if (xname == undefined) {
        right = "2%"
      } else {
        right = (xname.length * 2 + 3) + '%';
      }
      this.chart.setOption({
        // title: {
        //   text: title,
        //   left: 'left'
        // },
        xAxis: {
          name:xname,
          data: xdata,
          boundaryGap: false,
          axisTick: {
            show: false
          }
        },
        grid: {
          left: 10,
          right:right ,
          bottom: 20,
          top: 30,
          containLabel: true
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross'
          },
          padding: [5, 10],
          feature: {
            saveAsImage: { show: true, title: "oper_download" }
          }
        },
        yAxis: {
          name:yname,
          axisTick: {
            show: false
          }
        },
        legend: {},
        series: [{
          name: title,
          smooth: true,
          type: 'line',
          itemStyle: {
            normal: {
              color: '#3888fa',
              lineStyle: {
                color: '#3888fa',
                width: 2
              },
              areaStyle: {
                color: '#f3f8ff'
              }
            }
          },
          areaStyle: {},
          data: ydata,
          animationDuration: 2800,
          animationEasing: 'quadraticOut'
        }]
      })
    }
  }
}

</script>
