<template>
  <div>
    <div class="charts" ref="chart"></div>
  </div>
</template>

<script>
export default {
  name: "bar",
  data() {
    return {

    };
  },
  mounted() {
    this.setPie();
  },
  methods: {
    setPie() {
      const pieData = [{sex:'男性',num:'245678',per:'50%'},{sex:'女性',num:'245678',per:'50%'}]
      const chart = this.$refs.chart;
      if (chart) {
        const myChart = this.$echarts.init(chart);
        const option = {
          legend: {
            show: true,
            top: "30%",
            left: "7%",
            orient: "horizontal",
            itemGap: 200,
            itemWidth: 56,
            itemHeight: 5,
            textStyle: {
              color: "#fff",
            },
            formatter: (params) => {
              console.log(pieData);
              return  `{sex|`+ params + `}`;
            },
            rich:{
                sex:{
                    color:'red'
                }
            }
          },
          series: [
            {
              name: "title",
              type: "pie",
              zlevel: 3,
              hoverAnimation: false,
              legendHoverLink: false,
              radius: ["45%", "50%"],
              center: ["45%", "50%"],
              label: {
                show: false,
              },
              labelLine: {
                show: false,
                // showAbove:true,
                length: 30,
                length2: 50,
              },
              // itemStyle: {
              //     shadowBlur: 15,
              //     shadowColor: 'rgba(0, 0, 0, 0.3)',
              // },
              data: [
                {
                  value: 20,
                  name: "女性",
                  itemStyle: {
                    color: "rgba(255,43,133,1)",
                  },
                },
                {
                  value: 20,
                  name: "男性",
                  itemStyle: {
                    color: "rgba(5, 151, 252, 1)",
                  },
                },
              ],
            },
            {
              name: "",
              type: "pie",
              zlevel: 1,
              cursor: "default",
              hoverAnimation: false,
              legendHoverLink: false,
              radius: ["37%", "50%"],
              center: ["45%", "50%"],
              label: {
                show: false,
              },
              labelLine: {
                show: false,
              },
              tooltip: {
                show: false,
              },
              data: [
                {
                  value: 20,
                  name: "女性",
                  itemStyle: {
                    color: "rgba(255,43,133,0.16)",
                  },
                },
                {
                  value: 20,
                  name: "男性",
                  itemStyle: {
                    color: "rgba(5, 151, 252, 0.16)",
                  },
                },
              ],
            },
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
  width: 517px;
  height: 188px;
}
</style>
