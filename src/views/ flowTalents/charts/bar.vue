<template>
    <div>
    <div class="charts" ref="chart"></div>
    </div>
</template>

<script>
export default{
    name:'bar',
    data(){
        return{

        }
    },
    mounted(){
        this.test()
    },
    methods:{
        test() {
      const chart = this.$refs.chart;
      if (chart) {
        const myChart = this.$echarts.init(chart);
        const option = {
          xAxis: {
            type: "category",
            data: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
          },
          yAxis: {
            type: "value",
          },
          series: [
            {
              data: [120, 200, 150, 80, 70, 110, 130],
              type: "bar",
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
}
</script>

<style scoped>
.charts{
    width: 200px;
    height: 200px;
}
</style>
