<template>
  <div>
    <div class="charts" ref="chart"></div>
  </div>
</template>

<script>
export default {
  name: "bar",
  data() {
    return {};
  },
  mounted() {
    this.test();
  },
  methods: {
    test() {
      const chart = this.$refs.chart;
      if (chart) {
        const myChart = this.$echarts.init(chart);
        var data = [
          {
            name: "11",
            value: 565,
          },
          {
            name: "22",
            value: 878,
          },
          {
            name: "33",
            value: 813,
          },
          {
            name: "44",
            value: 800,
          },
          {
            name: "55",
            value: 800,
          },
          {
            name: "66",
            value: 800,
          },
          {
            name: "77",
            value: 800,
          },
        ];
        var xData = [],
          yData = [];
        var min = 0; // 最小值的定义
        data.map(function (a, b) {
          xData.push(a.name);
          if (a.value === 0) {
            yData.push(a.value + min);
          } else {
            yData.push(a.value);
          }
          // yData.push((Math.random(0,1) * 100).toFixed(0));
        });
        const option = {
          tooltip: {
            trigger: "axis",
            axisPointer: {
              type: "line",
              lineStyle: {
                opacity: 0,
              },
            },
            formatter: function (prams) {
              if (prams[0].data === min) {
                return "合格率：0%";
              } else {
                return "合格率：" + prams[0].data + "%";
              }
            },
          },
          legend: {
            data: ["接收量", "转出量"],
            show: true,
          },
          grid: {
            left: "0%",
            right: "10%",
            bottom: "5%",
            top: "20%",
            height: "80%",
            width:"90%",
            containLabel: true,
            z: 22,
          },
          xAxis: [
            {
              type: "category",
              gridIndex: 0,
              data: xData,
              axisTick: {
                alignWithLabel: true,
              },
              splitLine:{
                show:true,
                lineStyle: {
                  color: "#1C4666",
                },
              },
              axisLine: {
                lineStyle: {
                  color: "#0c3b71",
                },
              },
              axisLabel: {
                show: true,
                color: "rgb(170,170,170)",
                fontSize: 16,
              },
            },
          ],
          yAxis: [
            {
              type: "value",
              gridIndex: 0,
              splitNumber:6,
              splitLine: {
                show: true,
                lineStyle: {
                  color: "#1C4666",
                },
              },
              axisTick: {
                show: false,
              },
              min: min,
              max: 1200,
              axisLine: {
                lineStyle: {
                  color: "#0c3b71",
                },
              },
              axisLabel: {
                color: "rgb(170,170,170)",
                formatter: "{value}",
              },
            },
            // {
            //   type: "value",
            //   gridIndex: 0,
            //   min: min,
            //   max: 100,
            //   splitNumber: 6,
            //   splitLine: {
            //     show: true,
            //     lineStyle: {
            //       color: "#1C4666",
            //     },
            //   },
            //   axisLine: {
            //     show: false,
            //   },
            //   axisTick: {
            //     show: false,
            //   },
            //   axisLabel: {
            //     show: false,
            //   },
            //   splitArea: {
            //     show: true,
            //     areaStyle: {
            //       color: ["rgba(0, 102, 255, 0.04)", "rgba(0, 102, 255, 0.04)"],
            //     },
            //   },
            // },
          ],
          series: [
            {
              name: "接收量",
              type: "bar",
              barWidth: 8,
              xAxisIndex: 0,
              yAxisIndex: 0,
              itemStyle: {
                normal: {
                  borderRadius: [30, 30, 0, 0],
                  color: this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                    {
                      offset: 0,
                      color: "rgba(76,215,255,1)",
                    },
                    {
                      offset: 1,
                      color: "rgba(0,101,214,1)",
                    },
                  ]),
                },
              },
              data: yData,
              zlevel: 11,
            },
            {
              name: "转出量",
              type: "bar",
              barWidth: 8,
              xAxisIndex: 0,
              yAxisIndex: 0,
              itemStyle: {
                normal: {
                  borderRadius: [30, 30, 0, 0],
                  //   color: this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  //     {
                  //       offset: 0,
                  //       color: "#fff82e",
                  //     },
                  //     {
                  //       offset: 1,
                  //       color: "#deac00",
                  //     },
                  //   ]),
                  color: {              //好像不能够同时使用this.$echarts.graphic ,后设置的颜色会把前面的覆盖掉
                    type: "linear",
                    x: 0,
                    y: 0,
                    x2: 0,
                    y2: 1,
                    colorStops: [
                      {
                        offset: 0,
                        color: "#fff82e",
                      },
                      {
                        offset: 1,
                        color: "#deac00",
                      },
                    ],
                  },
                },
              },
              data: yData,
              zlevel: 11,
            },
            // {
            //     name: '背景',
            //     type: 'bar',
            //     barWidth: '50%',
            //     xAxisIndex: 0,
            //     yAxisIndex: 1,
            //     barGap: '-135%',
            //     data: [100, 100, 100, 100, 100, 100, 100],
            //     itemStyle: {
            //         normal: {
            //             color: 'rgba(255,255,255,0.1)'
            //         }
            //     },
            //     zlevel: 9
            // },
          ],
        };

        myChart.setOption(option);
        window.addEventListener("resize", function () {
          myChart.resize();
        });
      }
      this.$on("hook:destroyed", () => {
        window.removeEventListener("resize", function () {
          myChart.resize();
        });
      });
    },
  },
};
</script>

<style scoped>
.charts {
  width: 547px;
  height: 228px;
}
</style>
