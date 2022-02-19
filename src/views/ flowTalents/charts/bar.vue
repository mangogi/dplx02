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
    grid: {
        left: '5%',
        right: '5%',
        bottom: '5%',
        top: '10%',
        containLabel: true
    },
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'none'
        },
        formatter: function(params) {
            return params[0].name + '<br/>' +
                "<span style='display:inline-block;margin-right:5px;border-radius:10px;width:9px;height:9px;background-color:rgba(36,207,233,0.9)'></span>" +
                params[0].seriesName + ' : ' + Number((params[0].value.toFixed(4) / 10000).toFixed(2)).toLocaleString() + ' 万元<br/>'
        }
    },
    xAxis: {
        show: false,
        type: 'value'
    },
    yAxis: [{
        type: 'category',
        inverse: true,
        axisLabel: {
            show: true,
            textStyle: {
                color: '#58b5f5'
            },
        },
        splitLine: {
            show: false
        },
        axisTick: {
            show: false
        },
        axisLine: {
            show: false
        },
        data: ['60岁以上', '50～59', '40～49', '30～39', '20～29','20及以下']
    }, {
        type: 'category',
        inverse: true,
        axisTick: 'none',
        axisLine: 'none',
        show: true,
        axisLabel: {
            textStyle: {
                color: '#ffffff',
                fontSize: '12'
            },
            formatter: function(value) {
                if (value >= 10000) {
                    return (value / 10000).toLocaleString() + '万';
                } else {
                    return value.toLocaleString();
                }
            },
        },
        data: [50000000, 22000000, 10000000, 5000000, 1,5000000, 5000000]
    }],
    series: [{
            name: '金额',
            type: 'bar',
            zlevel: 1,
            itemStyle: {
                normal: {
                    borderRadius: 30,
                    // color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
                    //     offset: 0,
                    //     color: 'rgb(57,89,255,1)'
                    // }, {
                    //     offset: 1,
                    //     color: 'rgb(46,200,207,1)'
                    // }]),
                    color:'#58b5f5'
                },
            },
            barWidth: 12,
            data: [50000000, 22000000, 10000000, 5000000, 5000000, 5000000]
        },
        {
            name: '背景',
            type: 'bar',
            barWidth: 12,
            barGap: '-100%',
            data: [50000000, 50000000, 50000000, 50000000, 50000000, 50000000],
            itemStyle: {
                normal: {
                    color: 'rgba(24,31,68,1)',
                    borderRadius: 30,
                }
            },
        },
    ]
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
    width: 517px;
    height: 200px;
    margin-left: 40px;
}
</style>
