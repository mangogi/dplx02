<template>
  <div class="main">
    <div class="top">
      <div class="picker">新疆维吾尔自治区</div>
      <div class="title">流动人员档案情况分析</div>
      <div class="picker">日期选择器</div>
    </div>
    <div class="center">
      <top-box
        :dajsl2="dajsl2"
        :tit="'档案总存档'"
        :down="down"
        :up="up"
        :keys="'1'"
      >
      </top-box>
      <top-box
        :dajsl2="dajsl2"
        :tit="'档案接受量'"
        :down="down"
        :up="up"
        :keys="'2'"
      >
      </top-box>
      <top-box
        :dajsl2="dajsl2"
        :tit="'档案借阅量'"
        :down="down"
        :up="up"
        :keys="'3'"
      >
      </top-box>
      <top-box
        :dajsl2="dajsl2"
        :tit="'档案转出量'"
        :down="down"
        :up="up"
        :keys="'4'"
      >
      </top-box>
    </div>
    <div class="bottom">
      <div class="left">
        <div class="leftTop">
          <div class="titlebar pos">
            <div class="titletext">性别分布</div>
          </div>
          <pie class="charts1"></pie>
        </div>
        <div class="leftCenter"></div>
        <div class="leftBottom"></div>
      </div>
      <div class="center02">
        <div class="center02Top"></div>
        <div class="center02Bottom"></div>
      </div>
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
import Bar from "./part/bar.vue";
import TopBox from "./part/TopBox.vue";
import Pie from "./part/pie.vue"

export default {
  name: " MainPage",
  components: {
    Bar,
    TopBox,
    Pie,
  },
  data() {
    return {
      dajsl: 46199,
      dajsl2: [],
      up: "+2.56%",
      down: "-2.56%",
    };
  },
  mounted() {
    this.arrSet()
  },
  methods: {
    arrSet() {
      const ddd = this.dajsl.toString().split("");
      this.dajsl2 = ddd;
      console.log(this.dajsl2);
      if (this.dajsl2.length < 6) {
        if (this.dajsl2.length == 5) {
          this.dajsl2.unshift("0");
          console.log(this.dajsl2);
        }
        if (this.dajsl2.length == 4) {
          this.dajsl2.unshift("0", "0");
          console.log(this.dajsl2);
        }
        if (this.dajsl2.length == 3) {
          this.dajsl2.unshift("0", "0", "0");
          console.log(this.dajsl2);
        }
        if (this.dajsl2.length == 2) {
          this.dajsl2.unshift("0", "0", "0", "0");
          console.log(this.dajsl2);
        }
        if (this.dajsl2.length == 1) {
          this.dajsl2.unshift("0", "0", "0", "0", "0");
          console.log(this.dajsl2);
        }
      }
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
  background-image: url("../assets/imgs/01.png");
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

.titlebar{
  width: 517px;
	height: 51px;
	background: linear-gradient(to right, 
		rgba(0, 84, 188, 0.34), 
		rgba(0, 85, 188, 0));
	background-blend-mode: normal, 
		normal;
}
.pos{
  margin-left: 40px;
  margin-top: 13px;
}
.titletext{
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
.charts1{
  margin-left: 40px;
}
</style>
