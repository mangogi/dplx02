<template>
  <div
    id="regionCharts"
    ref="map"
    :style="{ width: '800px', height: '540px' }"
  ></div>
</template>

<script>
import mapJson from '../../../../static/json/xinjiang.json'
// import echarts from 'echarts'
// import 'echarts/map/js/china.js';
export default {
  name: 'mapChart',
  props: {
    mapData: {
      type: Array,
      default: () => {
        return [
          {
            count: 2,
            sourceLat: '42.50064453125', // 北纬
            sourceLng: '93.28025390625', // 东经
            name: '和田地区',
            targetName: '乌鲁木齐',
            targetId: '001',
          },
          {
            count: 6,
            sourceLat: '39.30064453125',
            sourceLng: '75.59025390625',
            targetName: '乌鲁木齐',
            name: '和田地区',
            targetId: '001',
          },
          {
            count: 8,
            sourceLat: '44.57064453125',
            sourceLng: '82.08025390625',
            name: '和田地区',
            targetName: '乌鲁木齐',
            targetId: '001',
          },
          {
            count: 10,
            sourceLat: '37.12064453125',
            sourceLng: '79.94025390625',
            name: '和田地区',
            targetName: '乌鲁木齐',
            targetId: '001',
          },
        ]
      },
    },
    destination: {
      type: Array,
      default: () => {
        return [87.364965353, 43.45224936]
      },
    },
    keys: {
      type: String,
      default: 'in',
    },
  },
  data() {
    return {
      chart: {},
      option: {},
      convertData: null,
      lineColor: '#1be7d4',
    }
  },
  watch: {
    keys: {
      handler(val, oldVal) {
        if (val) {
          this.drawMap(this.lineColor)
        }
      },
      deep: true,
      immediate: true,
    },
  },
  mounted() {
    this.drawMap()
  },
  created() {},
  methods: {
    drawMap() {
      console.log(this.destination)
      // 注册地图
      this.$echarts.registerMap('xinijang', mapJson) // 如果是js引入就不需要这一行了
      // 绘制地图
      this.chart = this.$echarts.init(document.getElementById('regionCharts'))
      // 该方法处理数据 决定箭头是往内还是外
      this.convertData = (data, des) => {
        let res = []
        for (let i = 0; i < data.length; i++) {
          let dataItem = data[i]
          let fromCoord = [dataItem.sourceLng, dataItem.sourceLat] // 起点
          let toCoord = des // 终点
          console.log(des)
          if (fromCoord && toCoord && this.keys == 'in') {
            res.push([
              {
                coord: fromCoord,
                value: dataItem.count,
              },
              {
                coord: toCoord,
              },
            ])
            this.lineColor = '#1be7d4'
          }
          if (fromCoord && toCoord && this.keys == 'out') {
            if (fromCoord && toCoord) {
              res.push([
                {
                  coord: toCoord, // 目的地
                  value: dataItem.count,
                },
                {
                  coord: fromCoord, // 出发地
                },
              ])
              this.lineColor = '#e7db1b'
            }
          }
        }
        return res
      }
      this.getOpt(this.lineColor)
    },
    getOpt(color) {
      this.option = {
        tooltip: {
          trigger: 'item',
          formatter: function(params) {
            console.log('tool', params)
          },
        },
        geo: {
          map: 'xinijang',
          aspectScale: 0.75,
          zoom: 1.18,
          roam: false,
          itemStyle: {
            normal: {
              areaColor: '#013c62',
              shadowColor: '#182f68',
              shadowOffsetX: 0,
              shadowOffsetY: 26, // 阴影 实现效果
            },
            emphasis: {
              areaColor: '#2ab8ff',
              borderWidth: 0,
              color: 'green',
              label: {
                show: true,
              },
            },
          },
        },
        series: [
          {
            type: 'lines',
            zlevel: 2,
            color: color, //线的颜色
            effect: {
              show: true,
              period: 4, // 箭头指向速度，值越小速度越快
              trailLength: 0.02, // 特效尾迹长度[0,1]值越大，尾迹越长重
              symbol: 'arrow', // 箭头图标
              symbolSize: 15, // 图标大小
            },
            lineStyle: {
              normal: {
                width: 1, // 尾迹线条宽度
                opacity: 1, // 尾迹线条透明度
                curveness: 0.1, // 尾迹线条曲直度
              },
            },
            data: this.convertData(this.mapData, this.destination),
          },
          {
            type: 'effectScatter',
            coordinateSystem: 'geo',
            zlevel: 7,
            rippleEffect: {
              // 涟漪特效
              period: 4, // 动画时间，值越小速度越快
              brushType: 'stroke', // 波纹绘制方式 stroke, fill
              scale: 8, // 波纹圆环最大限制，值越大波纹越大
            },
            label: {
              normal: {
                show: true,
                position: 'right', // 显示位置
                offset: [5, 0], // 偏移设置
                formatter: function(params) {
                  // 圆环显示文字
                  return params.value[2]
                },
                fontSize: 16,
              },
              emphasis: {
                show: true,
              },
            },
            symbol: 'circle',
            symbolSize: function(val) {
              return 5 + val[2] * 2 // 圆环大小
            },
            itemStyle: {
              normal: {
                show: false,
                color: color,
              },
            },
            data: this.mapData.map(function(dataItem) {
              return {
                name: '',
                value: [dataItem.sourceLng, dataItem.sourceLat, dataItem.count],
              }
            }),
          },
          // 被攻击点
          {
            type: 'effectScatter',
            coordinateSystem: 'geo',
            zlevel: 2,
            rippleEffect: {
              period: 4,
              brushType: 'stroke',
              scale: 8,
            },
            label: {
              normal: {
                show: true,
                position: 'right', // 显示位置
                offset: [5, 0], // 偏移设置
                formatter: function(params) {
                  // 圆环显示文字
                  return params.value[1]
                },
                fontSize: 16,
              },
              emphasis: {
                show: true,
              },
            },
            symbol: 'circle',
            symbolSize: function(val) {
              return 2 + val[2] * 2 // 圆环大小
            },
            itemStyle: {
              normal: {
                show: false,
                color: color,
              },
            },
            data: [
              {
                name: '乌鲁木齐市',
                value: ['33.43224936', '83.944965353', 10],
              },
            ],
          },
        ],
      }
      this.chart.setOption(this.option)
    },
    refreshChart() {
      this.chart.setOption(this.option)
    },
  },
}
</script>

<style scoped lang="less"></style>
