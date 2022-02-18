<template>
    <div class="charts" ref="chart"></div>
</template>

<script>
export default {
  name: "bar",
  props:{
    pieData:{
      type:Array,
      default:() => {
        return []
      }
    },
    bgUrl:{
      type:String,
      default:''
    },
    iconUrl:{
      type:String,
      default:''
    }

  },
  data() {
    return {

    };
  },
  mounted() {
    this.setPie();
  },
  methods: {
    setPie() {
      const pieData = this.pieData
      const chart = this.$refs.chart;
      if (chart) {
        const myChart = this.$echarts.init(chart);
        const option = {
        graphic: {  
          elements:[{
            type: 'image',
            left: '40%',
            top:'32%',
            z:3,
            style: {
            image: this.iconUrl,
            width: 56,
						height: 56
            },
          },
            {
            type: 'image',
            left: '32.5%',
            top:'15.8%',
            z:3,
            style: {
              image: require('../../../assets/imgs/bg.png'),
              width: 130,
						height: 130
            },
          },
          ]
        },
          legend: {
            show: true,
            top: "-10%",
            left: "7%",
            orient: "horizontal",
            itemGap: 160,
            itemWidth: 56,
            itemHeight: 5,
            textStyle: {
              color: "#fff",
            },
            align:"left",
            formatter: (params) => {
              console.log(pieData);
              let pieDataReverse = pieData.reverse()
              for(let i = 0; i < pieDataReverse.length; i++){
                console.log(params,pieDataReverse[i].name)
                if(pieDataReverse[i].name == params){
                  return '\n\n\n\n\n\n' + '{sex|'+pieDataReverse[i].name+'}' + '\n' +'{num|'+pieDataReverse[i].val + '}' + '{person|'+ '人'+ '}' + '\n' + '{pecent|' + pieDataReverse[i].per + '}'
                }
                
              }
            },
            textStyle:{
              color:'white',
              padding:[0,-60],
              rich:{
                sex:{
                    color:'white',
                    fontSize:16,
                    padding:[30,0,0,0]    //上 右 下 左
                },
                num:{
                  color:'white',
                  fontSize:28,
                  padding:[5,0]
                },
                person:{
                  fontSize:14,
                  padding:[5,0,0,10]
                },
                percent:{
                  fontSize:14
                }
            }
            }
            
          },
          series: [
            {
              name: "title",
              type: "pie",
              zlevel: 3,
              startAngle:270, //起始角度
              // hoverAnimation: false,
              // legendHoverLink: false,
              radius: ["52%", "60%"],
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
              data: [
               
                {
                  value: pieData[0].val,
                  name: pieData[0].name,
                  itemStyle: {
                    color: "rgba(5, 151, 252, 1)",
                  },
                },
                 {
                  value: pieData[1].val,
                  name: pieData[1].name,
                  itemStyle: {
                    color: "rgba(255,43,133,1)",
                  },
                },
              ],
            },
            {
              name: "",
              type: "pie",
              zlevel: 1,
              cursor: "default",
              startAngle:270, //起始角度
              // hoverAnimation: false,
              // legendHoverLink: false,
              radius: ["45%", "60%"],
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
                  value: pieData[0].val,
                  name: pieData[0].name,
                  itemStyle: {
                    color: "rgba(5, 151, 252, 0.16)",
                  },
                },
                {
                  value: pieData[1].val,
                  name: pieData[1].name,
                  itemStyle: {
                    color: "rgba(255,43,133,0.16)",
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
