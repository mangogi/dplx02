<template>
  <div class="pyramid" ref="pyramid"></div>
</template>

<script>
export default {
  name: "newPyramid",
  data() {
    return {};
  },
  mounted() {
    this.initCharts();
  },
  methods: {
    initCharts() {
      let datas = [
        { value: 20, name: "硕士及以上" },
        { value: 40, name: "本科" },
        { value: 60, name: "高中" },
        { value: 80, name: "中专及以下" },
        { value: 100, name: "专科" },
      ];
      let percentData = datas.forEach(item => { 
          let s = 0
          s += item.value
          })
      const pyramid = this.$refs.pyramid;
      let chart = this.$echarts.init(pyramid);
      var colorList = ["#8bfffd", "#3de6e3", "#3bbcd9", "#378acc", "#345ec0"];
      let option = (option = {
        color: colorList,
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c}",
        },

        series: [
          {
            name: "",
            type: "funnel",
            left: "19%",
            width: "35%",
            sort: "ascending",
            gap: 0,
            label: {
              normal: {
                formatter: "{b}",
                fontSize: 14,
                color: "#fff",
                formatter: function (params) {
                  return "{name|" + params.name + "}" + "\n" + params.value  
                },
                rich: {
                  name: {
                    color: "#5cbcff",
                  },
                },
              },
            },
            labelLine: {
              normal: {
                show: true,
                length: 70,
              },
            },
            itemStyle: {
              normal: {
                opacity: 1,
                borderColor: "rgba(9,20,36,0)",
              },
              rotate: -32,
            },
            data: datas
          },
          {
            name: "",
            type: "funnel",
            left: "19%",
            width: "35%",
            maxSize: "70%",
            sort: "ascending",
            gap: 0,
            label: {
              show: false,
            },
            itemStyle: {
              normal: {
                opacity: 1,
                borderColor: "rgba(9,20,36,0)",
                borderWidth: 0,
                shadowBlur: 5,
                shadowOffsetX: 5,
                shadowOffsetY: 0,
                shadowColor: "rgba(0, 0, 0, 1)",
              },
            },

            data: datas,
          },
        ],
      });

      chart.setOption(option);
    },
  },
};
</script>

<style lang="less" scoped>
.pyramid {
  width: 500px;
  height: 270px;
}
</style>