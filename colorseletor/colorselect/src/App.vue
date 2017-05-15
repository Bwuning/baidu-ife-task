<template>
  <div id="app">
    <input v-model="color0x" type="text" v-on:click="showSelector" v-on:click.stop>
    <div class="selector" v-on:mousedown.prevent  v-on:click.stop.prevent v-bind:class="{hidden:unShow}">
      <div v-on:mousedown.prevent v-on:click.prevent class="left">
        <div v-on:mousedown.prevent v-on:click.prevent v-bind:style="{background: 'linear-gradient(to right,#ffffff, '+ Hcolor +')'}" class="color table"></div>
        <div v-on:mousedown.prevent v-on:click.prevent class="black table"></div>
        <div class="point" v-bind:style="{left: pointLeft, top: pointTop }" v-on:mousedown="dragPoint"></div>
      </div>
      <div class="colorful" >
        <div v-bind:style="{top:sliderTop}" class="slider" v-on:mousedown="dragSlider"></div>
      </div>
    </div>
  </div>
</template>

<script>
function ColorHSB(H,S,B){
  this.h = H;
  this.s = S;
  this.b = B;
}

ColorHSB.prototype.to0x=function(){
  var v = (this.b/200)
  var a = []
    var h1 = Math.floor(this.h *6 /200.0001)
    var f = (this.h *6 /200.0001) - h1
    var p = (this.b/200)*(1-(this.s/200))
    var q = (this.b/200)*(1-f*(this.s/200))
  var t = (this.b/200)*(1-(1-f)*(this.s/200))
  switch (h1){
    case 0:
      a = [v,t,p];
      break;
    case 1:
      a = [q,v,p];
      break;
    case 2:
      a = [p,v,t];
      break;
    case 3:
      a =[p,q,v];
      break;
    case 4:
      a = [t,p,v];
      break;
    case 5:
      a = [v,p,q];
      break;
  }
    function rgbTo0x(arr){
      function numToStr(num){
        var str = Math.floor(num*255.999999).toString(16)
        if(str.length == 0){
          str = "00"
        }
        if(str.length == 1){
          str = "0"+ str
        }
        return str
      }

      return (numToStr(arr[0])+numToStr(arr[1])+numToStr(arr[2]))  
    }
  return rgbTo0x(a);
}

function Oxtohsb(str){
  // 将 str 长度增减为 6 
  { 
    if(str.length >= 6)
    {
      this.do = true
      str = str.substring(0,6) 
      // 测试 str 是否为 十六进制数字
      {
        if(/^[0-9a-fA-F]{3,6}$/.test(str)){
          // ture
        }else{
          //false
          this.do =false
          str = 'ffffff'
        } 
      }
      // 将 str 转换 为 rgb
      {
        var r = parseInt(str.substring(0,2),16)/255
        var g = parseInt(str.substring(2,4),16)/255
        var b = parseInt(str.substring(4,6),16)/255
      }
      // rgb 转换为 hsb
      {
        //计算 max 和 min
        {
          var max = r
          var min = r
          if(g > max){
            max = g
          }else{
            min = g
          }
          if(b > max){
            max = b
          }else if(b < min){
            min = b
          }
        }
        //计算 hsb
        {
          //计算 v
          {
            this.v = max
          }
          //计算 s
          {
            if(max == 0){
              this.s = 0
            }else{
              this.s = 1 - (min/max)
            }
          }
            //计算 h
          {
            if(max == min){

              this.h = 0

            }else if((max == r)&&(g >= b)){

              this.h = (g-b)/((max-min)*6)

            }else if((max == r)&&(g < b)){

              this.h = 1+(g-b)/((max-min)*6)

            }else if(max == g){

              this.h = (b-r)/((max-min)*6)+(1/3)

            }else if(max == b){

              this.h = (r-g)/((max-min)*6)+(2/3)
            }
          }
        }
      } 
    }else{
      this.do =false
    }
  }
  
}



export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      sliderY: 104,
      pointX: 0,
      pointY: 0,
      unShow: true,
      message:{
        color:"66ccff"
      }
    }
  },
  methods:{
    showSelector(){
      if(document.body.onclick){
        document.body.onclick()
      }

      this.$data.unShow = false
      let obj = this
      console.log("1")
      document.body.onclick = function(){
        obj.$data.unShow = true
        console.log("2")
        document.body.onclick=null
      }
    },
    dragSlider(ev){
      let vueObj = this;
      let slider = ev.currentTarget
      let yBegin = ev.clientY
      let yNow = vueObj.$data.sliderY
      document.onmousemove = function(ev){
        let tempY = yNow + ev.clientY - yBegin
        if(tempY<=0){
          vueObj.$data.sliderY = 0
        }else if(tempY >=200){
          vueObj.$data.sliderY = 200
        }else{
          vueObj.$data.sliderY = tempY 
        }
      }
      document.onmouseup = function(){
        document.onmousemove = null
        document.onmouseup = null
      }
    },
    dragPoint(ev){
      let vueObj = this;
      let point = ev.currentTarget
      let xBegin = ev.clientX
      let yBegin = ev.clientY
      let xNow = vueObj.$data.pointX
      let yNow = vueObj.$data.pointY
      document.onmousemove = function(ev){
        let tempX = xNow + ev.clientX - xBegin
        let tempY = yNow + ev.clientY - yBegin
        if(tempX<=0){
          vueObj.$data.pointX = 0
        }else if(tempX >=200){
          vueObj.$data.pointX = 200
        }else{
          vueObj.$data.pointX = tempX
        }
        if(tempY<=0){
          vueObj.$data.pointY = 0
        }else if(tempY >=200){
          vueObj.$data.pointY = 200
        }else{
          vueObj.$data.pointY = tempY 
        }
      }
      document.onmouseup = function(){
        document.onmousemove = null
        document.onmouseup = null
      }
    }
  },
  computed: {
    sliderTop: function () {
      let str = ''
      str= (this.$data.sliderY-4)+'px'
      return str
    },
    isHidden:function(){
      let a = ''
      this.$data.isShow? a='hidden':a=''
      return a
    },
    pointLeft: function () {
      let strX = ''
      strX= (this.$data.pointX-6)+'px'
      return strX
    },
    pointTop: function () {
      let strY = ''
      strY= (this.$data.pointY-6)+'px'
      return strY
    },
    Hcolor:function(){
      let color = new ColorHSB(this.$data.sliderY,200,200)
      return "#"+color.to0x()
    },
    color0x: {
      get:function(){
        let color = new ColorHSB(this.$data.sliderY,this.$data.pointX,(200-this.$data.pointY))
        if(!this.unShow){
          this.message.color = color.to0x()
        }
        return color.to0x()
      },
      set:function(newValue){
        var temp = new Oxtohsb(newValue)
        if(temp.do){
          this.$data.sliderY=temp.h*200
          this.$data.pointX =temp.s*200
          this.$data.pointY =(1-temp.v)*200
          this.message.color = newValue
        }
      }
    }
  },
  mounted(){
    var temp = new Oxtohsb(this.message.color)
        if(temp.do){
          this.$data.sliderY=temp.h*200
          this.$data.pointX =temp.s*200
          this.$data.pointY =(1-temp.v)*200
        }
    console.log(this.message.color)
  }
}
</script>

<style>
@font-face{
  font-family:"Frutiger";
  /*为引入的字体命名*/
  src:url(http://ohk880ltu.bkt.clouddn.com/Frutiger%20LT%2045%20Light.ttf);
  /*定义要引入字体文件的路径*/
}
.colorseletor{
  width:100%;height:100%;position: relative;background-color: #ffffff;
}
.hidden{
  display:none!important;
}
#app {
  display: inline-block;left: 45%;top: 30%;transform: translate(-45%,-30%);position: absolute;
}
.selector{
  position: absolute;
  left: 0px;
  border: #838383 solid 1px;
  padding: 2px;display: inline-block;
  font-size: 0;
  top: 35px;
  width: 222px;height: 200px;
  z-index: 998;
}
.colorful{
  width:20px;height: 200px;display: inline-block;position: relative;background-image: url('./png/colorselect.png');background-size: 100%;
}

.left{
  width: 200px;height: 200px;display: inline-block;position: relative;margin-right: 2px;
}
.color{
/*  background: -webkit-linear-gradient(left, #ffffff, hsl(120,65%,75%));
  background: -o-linear-gradient(left, #ffffff, hsl(120,65%,75%));
  background: linear-gradient(to right,#ffffff, hsl(120,65%,75%));*/
}
.table{
  width: 100%;height: 100%;position: absolute;
}
.slider{
  position: absolute;height: 8px;width: 20px;outline: #838383 solid 1px;background-color: white;z-index: 999;cursor:pointer;
}
.black{
  background: -webkit-linear-gradient(rgba(0,0,0,0), rgba(0,0,0,1));
  background: -o-linear-gradient(rgba(0,0,0,0), rgba(0,0,0,1));
  background: linear-gradient(rgba(0,0,0,0), rgba(0,0,0,1));
}
.point{
  position: absolute;height: 10px;width: 10px;border: #838383 solid 1px;background-color: white;z-index: 999;cursor:pointer; border-radius: 5px;
}
body{
  position:fixed;width: 100%;height: 100%;
}
</style>
