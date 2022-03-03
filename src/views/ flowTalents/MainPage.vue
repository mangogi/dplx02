<template>
  <screen-adapter>
  <div class="main">
    <!-- 页面顶部 -->
    <div class="top">
      <div class="picker">新疆维吾尔自治区</div>
      <div class="title">流动人员档案情况分析</div>
      <div class="picker">日期选择器</div>
    </div>
    <!-- 页面中部四个方块部分 -->
    <div class="center">
      <top-box
        :num="dajslArr"
        title="档案总存档"
        :down="down"
        :up="up"
        :imgurl="allImg"
      >
      </top-box>
      <top-box
        :num="dajslArr"
        title="档案接受量"
        :down="down"
        :up="up"
        :imgurl="receiveImg"
      >
      </top-box>
      <top-box
        :num="dajslArr"
        title="档案借阅量"
        :down="down"
        :up="up"
        :imgurl="rendImg"
      >
      </top-box>
      <top-box
        :num="dajslArr"
        title="档案转出量"
        :down="down"
        :up="up"
        :imgurl="outImg"
      >
      </top-box>
    </div>
    <!-- 四个方块以下部分 -->
    <div class="bottom">
      <!-- 左边三个图表 -->
      <div class="left">
        <!-- 性别分布饼图 -->
        <div class="leftTop">
          <div class="titlebar pos">
            <div class="titletext">性别分布</div>
          </div>
          <!-- 性别分布饼图 -->
          <pie
            class="chartsSex chartsWidth"
            :pieData="sexData"
            :iconUrl="'xingbietongji'"
            :outColor="sexColorOne"
            :innerColor="sexColorTwo"
            v-if="flag"
          ></pie>
        </div>
        <div class="leftCenter">
          <div class="titlebar pos">
            <div class="titletext">年龄分布</div>
          </div>
          <!-- 年龄分布条形图 -->
          <bar class="chartsAge chartsWidth" :barData="ageData"></bar>
        </div>
        <div class="leftBottom">
          <div class="titlebar pos">
            <div class="titletext">学历分布</div>
          </div>
          <!-- 学历分布漏斗图 -->
          <pyramid></pyramid>
        </div>
      </div>

      <!-- 中间地图部分 -->
      <div class="mapBox">
        <div class="mapTop">
          <br />
        </div>
        <div class="mapBottom">
          <p>转移排行</p>
          <div class="panel_box">
            <panel :provinceData="provinceData[0]" :imgurl="'location-full'"></panel>
          <panel :provinceData="provinceData[1]" :imgurl="'location-full-2'"></panel>
          <panel :provinceData="provinceData[2]" :imgurl="'location-full-3'"></panel>
          <panel :provinceData="provinceData[3]" :imgurl="''" ></panel>
          <panel :provinceData="provinceData[4]" :imgurl="''" ></panel>
          <panel :provinceData="provinceData[5]" :imgurl="''"></panel>
          </div>
          
        </div>
      </div>
      <!-- 右边三个图表 -->
      <div class="right">
        <div class="rightTop">
          <div class="titlebar pos">
            <div class="titletext">存档性质分析</div>
          </div>
          <!-- 存档性质分析饼图 -->
          <pie
            class="chartsSex chartsWidth"
            :pieData="natureData"
            :iconUrl="'存档性质分析'"
            :outColor="natureColorOne"
            :innerColor="natureColorTwo"
            v-if="flag"
          ></pie>
        </div>
        <div class="rightCenter">
          <div class="titlebar pos">
            <div class="titletext">存档名族分析</div>
          </div>
          <!-- 存档名族分析饼图 -->
          <pie
            class="chartsSex chartsWidth"
            :pieData="nationData"
            :iconUrl="'存档民族分析'"
            :outColor="nationColorOne"
            :innerColor="nationColorTwo"
            v-if="flag"
          ></pie>
        </div>
        <div class="rightBottom">
          <div class="titlebar pos">
            <div class="titletext">近六个月档案接收和转出情况</div>
          </div>
          <!-- 近6个月档案接收和转出情况 -->
          <vertical-bar
            v-if="showBar"
            class="chartsSex chartsWidth"
            :barData="fileData"
          ></vertical-bar>
        </div>
      </div>
    </div>

    <!-- <bar></bar> -->
  </div>
  </screen-adapter>
</template>

<script>
import Bar from "./charts/bar.vue";
import TopBox from "./charts/TopBox.vue";
import Pie from "./charts/pie.vue";
import VerticalBar from "./charts/verticalBar.vue";
import Panel from "./charts/panel.vue";
import ScreenAdapter from './charts/ScreenAdapter.vue'
import pyramid from './charts/newPyramid.vue'

// 饮用component组件 用@
export default {
  name: " MainPage",
  components: {
    Bar,
    TopBox,
    Pie,
    VerticalBar,
    Panel,
    ScreenAdapter,
    pyramid,
  },
  data() {
    return {
      dajsl: 46199, //档案接收量
      dajslArr: [], //档案接收量数组
      up: "+2.56%", //增长比例
      down: "-2.56%", //下降比例
      allImg: "", //档案总存档图标路径
      receiveImg: "", //档案接收量图标路径
      rendImg: "", //档案租借量图标路径
      outImg: "", //档案转出量图标路径
      sexData: [
        { name: "男性", val: "245678", per: "50.00%" },
        { name: "女性", val: "240078", per: "50.00%" },
      ], //性别饼图数据
      nationData: [
        { name: "汉族", val: "245678", per: "50.00%" },
        { name: "少数名族", val: "240078", per: "50.00%" },
      ], //民族饼图数据
      natureData: [
        { name: "个人存档", val: "245678", per: "50.00%" },
        { name: "企业存档", val: "180078", per: "50.00%" },
      ], //性质饼图数据
      bgUrl: "", //
      sexUrl: "",
      nationUrl: "",
      natureUrl: "",
      sexColorOne: [],
      sexColorTwo: [],
      nationColorOne: [],
      nationColorTwo: [],
      natureColorOne: [],
      natureColorTwo: [],
      flag: false, //为了解决页面加载完成了 但是数据没有传到子组件的问题
      ageData: [
        { name: "60岁以上", value: "1234", per: "12%" },
        { name: "50～59", value: "1234", per: "12%" },
        { name: "40～49", value: "1234", per: "12%" },
        { name: "30～39", value: "1234", per: "12%" },
        { name: "20～29", value: "1234", per: "12%" },
        { name: "20及以下", value: "1234", per: "12%" },
      ],
      fileData: [],
      showBar: false,
      provinceData:[
        {name:'四川省',value:0.63,num:'8.8万人'},
        {name:'天津市',value:0.63,num:'8.8万人'},
        {name:'湖南省',value:0.63,num:'8.8万人'},
        {name:'北京市',value:0.63,num:'8.8万人'},
        {name:'上海市',value:0.63,num:'8.8万人'},
        {name:'江苏省',value:0.63,num:'8.8万人'},
      ],
      redlogo:'',
      yellowlogo:'',
      greenlogo:'',
      showPanel:false,
    };
  },
  mounted() {
    this.arrSet();
    this.imgUrlSet();
    this.setColor();
    this.setBarData();
  },
  methods: {
    /**
     * 用于处理数据不满六位，头部添零
     */
    arrSet() {
      const counts = this.dajsl.toString().split(""); //将数字转位数组
      this.dajslArr = counts;
      if (this.dajslArr.length < 6) {
        if (this.dajslArr.length == 5) {
          this.dajslArr.unshift("0");
        }
        if (this.dajslArr.length == 4) {
          this.dajslArr.unshift("0", "0");
        }
        if (this.dajslArr.length == 3) {
          this.dajslArr.unshift("0", "0", "0");
        }
        if (this.dajslArr.length == 2) {
          this.dajslArr.unshift("0", "0", "0", "0");
        }
        if (this.dajslArr.length == 1) {
          this.dajslArr.unshift("0", "0", "0", "0", "0");
        }
      }
    },
    /**
     * 处理图像路径 方便传参
     */
    imgUrlSet() {
      // topBox 组件的图片
      this.allImg = require("../../assets/imgs/档案总存档.png");
      this.receiveImg = require("../../assets/imgs/档案接受量.png");
      this.rendImg = require("../../assets/imgs/档案借阅量.png");
      this.outImg = require("../../assets/imgs/档案转出量.png");

      // pie组件的图片路径处理
      this.sexUrl = require("../../assets/imgs/xingbietongji.png");
      this.nationUrl = require("../../assets/imgs/存档民族分析.png");
      this.natureUrl = require("../../assets/imgs/存档性质分析.png");

    },
    /**
     * 设置饼图的颜色
     */
    setColor() {
      this.sexColorOne = ["rgba(5, 151, 252,1)", "rgba(255, 43, 133,1)"]; // out
      this.sexColorTwo = ["rgba(5, 151, 252, 0.16)", "rgba(255, 43, 133,0.16)"]; //in
      this.natureColorOne = ["rgba(0, 217, 153,1)", "rgba(255, 228, 0,1)"];
      this.natureColorTwo = [
        "rgba(0, 217, 153, 0.16)",
        "rgba(255, 228, 0,0.16)",
      ];
      this.nationColorOne = ["rgba(18, 91, 255, 1)", "rgba(26, 251, 255,1)"];
      this.nationColorTwo = ["rgba(18, 91, 255,0.16)","rgba(26, 251, 255,0.16)",];

      this.flag = true;
    },
    setBarData() {
      this.fileData = [
        {
          name: "20210302",
          in: 565,
          out: 333,
        },
        {
          name: "20210402",
          in: 565,
          out: 333,
        },
        {
          name: "20210402",
          in: 565,
          out: 333,
        },
        {
          name: "20210402",
          in: 565,
          out: 333,
        },
        {
          name: "20210402",
          in: 565,
          out: 333,
        },
        {
          name: "20210402",
          in: 565,
          out: 333,
        },
        {
          name: "20210402",
          in: 565,
          out: 333,
        },
      ],
        this.$nextTick(() => {
          this.showBar = true;
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
/*  页面样式*/
.main {
  width: 1920px;
  height: 1080px;
  background-color: #081c3b;
}
/* 标题部分 */
.top {
  width: 1920px;
  height: 83px;
  background-image: url("../../assets/imgs/01.png");
  background-repeat: no-repeat;
  margin-left: 10px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.title {
  /* background-image: url("../assets/imgs/titlle01.png");
  background-repeat: no-repeat; */
  z-index: 22;
  width: 300px;
  height: auto;
  font-family: SourceHanSansCN-Bold;
  font-size: 28px;
  line-height: 30px;
  font-weight: bold;
  font-stretch: normal;
  letter-spacing: 1px;
  color: #ffffff;
  background: linear-gradient(to bottom, white, #44a5eb);
  -webkit-background-clip: text;
  color: transparent;
  margin-top: 14px;
}
.picker {
  width: 223px;
  height: 33px;
  text-align: center;
  font-family: MicrosoftYaHei;
  font-size: 16px;
  font-weight: normal;
  font-stretch: normal;
  line-height: 80px;
  letter-spacing: 1px;
  color: #5db1ff;
}
/* 中间四个框部分 */
.center {
  margin-left: 40px;
  margin-top: 24px;
  display: flex;
  flex-direction: row;
}
/* 下面部分 */
.bottom {
  display: flex;
  flex-direction: row;
  width: 100%;
  height: 500px;
}
.left {
  width: 540px;
  height: 460px;
}
.titlebar {
  width: 517px;
  height: 51px;
  background: linear-gradient(
    to right,
    rgba(0, 84, 188, 0.34),
    rgba(0, 85, 188, 0)
  );
  background-blend-mode: normal, normal;
}
.pos {
  margin-left: 40px;
  margin-top: 13px;
}
.titletext {
  height: 16px;
  font-family: MicrosoftYaHei;
  font-size: 16px;
  font-weight: normal;
  font-stretch: normal;
  line-height: 50px;
  letter-spacing: 1px;
  color: #aee0ff;
  margin-left: 20px;
}
.chartsSex {
  margin-left: 40px;
}

.chartsWidth {
  width: 517px;
}
/* 地图部分 */
.mapBox {
  width: 756px;
  margin-left: 31px;
  color: white;
}
.mapTop {
  height: 640px;
}
.mapBottom {
  display: flex;
  flex-direction: column;
  height: 170px;
  font-size: 16px;
  color: #5ebdff;
}
.panel_box{
  height: 176px;
  display: flex;
  flex-direction: row;
}
/* 右边三个图表 */
.right {
  width: 540px;
  margin-left: 31px;
}
</style>
