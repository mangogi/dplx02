<template>
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
          <pie class="chartsSex"></pie>
        </div>
        <div class="leftCenter"></div>
        <div class="leftBottom"></div>
      </div>
      <!-- 中间地图部分 -->
      <div class="mapBox">
        <div class="mapTop"></div>
        <div class="mapBottom"></div>
      </div>
      <!-- 右边三个图表 -->
      <div class="right">
        <div class="rightTop"></div>
        <div class="rightCenter"></div>
        <div class="rightBottom"></div>
      </div>
    </div>

    <!-- <bar></bar> -->
  </div>
</template>

<script>
import Bar from "./charts/bar.vue";
import TopBox from "./charts/TopBox.vue";
import Pie from "./charts/pie.vue";

export default {
  name: " MainPage",
  components: {
    Bar,
    TopBox,
    Pie,
  },
  data() {
    return {
      dajsl: 46199,   //档案接收量
      dajslArr: [],     //档案接收量数组
      up: "+2.56%", //增长比例
      down: "-2.56%", //下降比例
      allImg: "", //档案总存档图标路径
      receiveImg:"",  //档案接收量图标路径
      rendImg:"",     //档案租借量图标路径
      outImg:"",    //档案转出量图标路径

    };
  },
  mounted() {
    this.arrSet();
    this.imgUrlSet()
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
      this.allImg = require("../../assets/imgs/档案总存档.png");
      this.receiveImg = require("../../assets/imgs/档案接受量.png");
      this.rendImg = require("../../assets/imgs/档案借阅量.png");
      this.outImg = require("../../assets/imgs/档案转出量.png");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
  font-weight: bold;
  font-stretch: normal;
  letter-spacing: 1px;
  color: #ffffff;
  box-shadow: 0px 2px 3px 0px rgba(0, 15, 57, 0.63);
  background: linear-gradient(to bottom, white, #44a5eb);
  -webkit-background-clip: text;
  color: transparent;
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
</style>
