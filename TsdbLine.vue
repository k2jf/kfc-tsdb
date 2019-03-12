<template>
  <div class="chart-container" ref="tsdbLineChart" />
</template>

<script>
import Echarts from 'echarts'
export default {
  name: 'TsdbLine',
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
        },
        xAxis: {
          type: 'time',
          splitLine: {
            show: false
          }
        },
        yAxis: {
          type: 'value',
          boundaryGap: [0, '100%'],
          splitLine: {
            show: false
          }
        },
        series: [{
          name: '模拟数据',
          type: 'line',
          showSymbol: false,
          hoverAnimation: false,
          data: []
        }]
      },
      now: null,
      oneDay: null,
      value: null,
      data: []
    }
  },
  mounted () {
    this.now = +new Date(1997, 9, 3)
    this.oneDay = 24 * 3600 * 1000
    this.value = Math.random() * 1000
    for (var i = 0; i < 30; i++) {
      this.data.push(this.randomData())
    }
    this.init()
  },
  methods: {
    // 初始化图表
    init () {
      this.tsdbLineChart = Echarts.init(this.$refs.tsdbLineChart)
      window.onresize = this.tsdbLineChart.resize
      setInterval(() => {
        for (let i = 0; i < 1; i++) {
          this.data.shift()
          this.data.push(this.randomData())
        }

        this.tsdbLineChart.setOption({
          ...this.option,
          series: [{
            type: 'line',
            data: this.data
          }]

        })
      }, 1000)
    },
    randomData () {
      this.now = new Date(+this.now + this.oneDay)
      this.value = this.value + Math.random() * 21 - 10
      return {
        name: this.now.toString(),
        value: [
          [this.now.getFullYear(), this.now.getMonth() + 1, this.now.getDate()].join('/'),
          Math.round(this.value)
        ]
      }
    }
  },
  beforDestory () {
    this.tsdbLineChart.clear()
  }
}
</script>

<style lang="css" scoped>
.chart-container {
  width: 100%;
  height: 600px;
}
</style>
