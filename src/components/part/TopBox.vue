<template>
  <div id="topbox">
    <div class="box01">
        <img :src="imgs" class="left01" />
        <div class="center01">
          <p class="text01">{{tit}}</p>
          <div class="counts">
            <div class="number01" v-for="(item,index) in dajsl2" :key="index">
              <span class="font">{{ item }}</span>
            </div>
            <span class="danwei">家</span>
          </div>
        </div>
        <div class="right01">
          <div>
            <span>同比</span>
            <span class="down">{{down}}</span>
          </div>
          <div>
            <span>同比</span>
            <span class="up">{{up}}</span>
          </div>
        </div>
      </div>
  </div>
</template>

<script>

export default {
  name: 'TopBox',
  props:['dajsl2','imgurl','tit','up','down','keys'],
  data(){
      return{
        imgs:'',
      }
  },
  mounted(){
      this.imgUrlSet()
  },
  methods:{
    imgUrlSet(){
      if(this.keys == '1'){
          this.imgs = require('../../assets/imgs/档案总存档.png')
      }
      if(this.keys == '2'){
          this.imgs = require('../../assets/imgs/档案接受量.png')
      }
      if(this.keys == '3'){
          this.imgs = require('../../assets/imgs/档案借阅量.png')
      }
      if(this.keys == '4'){
          this.imgs = require('../../assets/imgs/档案转出量.png')
      }
      
      console.log(this.imgs)
    }
  },
}
</script>

<style>
.box01 {
  width: 447px;
  height: 111px;
  background-image: url("../../assets/imgs/box01.png");
  margin-right: 20px;
  display: flex;
  flex-direction: row;
}
.left01 {
  margin-top: 31px;
  margin-left: 33px;
  width: 48px;
  height: 56px;
}
.center01 {
  margin-left: 17px;
  width: 200px;
  height: 100px;
  display: flex;
  flex-direction: column;
}
.number01 {
  width: 30px;
  height: 40px;
 background-color: #0043b9;
	border-radius: 2px;
	border: solid 1px #274d8e;
  margin-right: 3px;
  text-align: center;
}
.font {
  font-family: Impact;
  font-size: 28px;
  font-weight: normal;
  font-stretch: normal;
  letter-spacing: 1px;
  line-height: 40px;
  /* margin-left: 8px; */
  background: linear-gradient(to bottom, #ffffff, #44a5eb);
  -webkit-background-clip: text;
  color: transparent;
}
.text01 {
  width: 70px;
  height: 14px;
  font-family: PingFang-SC-Regular;
  font-size: 14px;
  font-weight: normal;
  font-stretch: normal;
  line-height: 20px;
  letter-spacing: 0px;
  color: #ffffff;
}
.danwei{
  width: 14px;
	height: 12px;
	font-family: MicrosoftYaHei;
	font-size: 14px;
	font-weight: normal;
	font-stretch: normal;
	line-height: 70px;
	letter-spacing: 0px;
	color: #a5a7b1;
  margin-left: 9px;
}
.counts {
  display: flex;
  flex-direction: row;
}
.right01{
  width: 100px;
	height: 38px;
	font-family: MicrosoftYaHei;
	font-size: 14px;
  display: flex;
  flex-direction: column;
  color: #a5a7b1;
  margin-top: 46px;
  margin-left: 24px;
}
.up{
	color: #ff2639;
  /* margin-left: 1px; */
}
.down{
	color: #3ef9bf;
  margin-left: 0px;
}
</style>
