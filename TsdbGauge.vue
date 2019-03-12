<template>
  <div class="chart-container" ref="tsdbGaugeChart" />
</template>

<script>
import Echarts from 'echarts'
export default {
  name: 'TsdbGauge',
  props: {
    queryParams: {
      type: Object,
      required: false,
      default () {
        return {}
      }
    }
  },
  data () {
    return {
      option: {
        tooltip: {
          formatter: '{a} <br/>{b} : {c}%'
        },
        toolbox: {
          feature: {
            dataZoom: {},
            restore: {},
            saveAsImage: {}
          }
        }
      }
    }
  },
  watch: {
  },
  created () {
  },
  mounted () {
    this.init()
  },
  methods: {
    // 初始化图表
    init () {
      this.tsdbGaugeChart = Echarts.init(this.$refs.tsdbGaugeChart)
      this.tsdbGaugeChart.showLoading({ text: '加载中...' })
      setInterval(this.showTimeSeriesChart, 1000)
      window.onresize = this.tsdbGaugeChart.resize
    },
    //
    showTimeSeriesChart () {
      let option = {
        ...this.option,
        series: this.getSeriesData()
      }
      this.tsdbGaugeChart.hideLoading()
      this.tsdbGaugeChart.setOption(option)
    },
    getSeriesData () {
      return [
        {
          name: 'tsdb',
          type: 'gauge',
          detail: { formatter: '{value}%' },
          data: [{ value: (Math.random() * 100).toFixed(2) - 0, name: '测试数据' }]
        }
      ]
    }
  },
  beforDestory () {
    this.tsdbGaugeChart.clear()
  }
}
</script>

<style lang="css" scoped>
.chart-container {
  width: 100%;
  height: 600px;
}
</style>
