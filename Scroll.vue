<template>
  <div class="wrapper">
    <div class="menuWrapper" ref="menuWrapper">
      <ul class="menuContent">
        <li class="menuList" :class="{active:currentIndex===index}" v-for="(menu,index) in menus" :key="menu.id" @click="selectMenu(index,$event)">{{menu.title}}</li>
      </ul>
    </div>
    <div class="foodWrapper" ref="foodWrapper">
      <ul class="foodContent">
        <li class="foodList" v-for="food in foods" :key="food.id">{{food.content}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
export default {
  data () {
    return {
      menus:[
        {
          id:'01',
          title:'menu1'
        },
        {
          id:'02',
          title:'menu2'
        },
        {
          id:'03',
          title:'menu3'
        },
        {
          id:'04',
          title:'menu4'
        },
        {
          id:'05',
          title:'menu5'
        }
      ],
      foods:[
        {
          id:'01',
          content:'content1'
        },
        {
          id:'02',          
          content:'content2'
        },
        {
          id:'03',
          content:'content3'
        },
        {
          id:'04',
          content:'content4'
        },
        {
          id:'05',
          content:'content5'
        }
      ],
      listHeight:[0],
      scrollY:0
    };
  },
  methods:{
    initScroll(){
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click:true
      })
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {
        click:true,
        probeType:3 // BScroll滚动时，获取实时位置，类似探针效果
      })
      this.foodScroll.on('scroll',(pos)=>{
        this.scrollY=Math.abs(Math.round(pos.y))
      })
    },
    calculateHeight(){
      let foodList=this.$refs.foodWrapper.getElementsByClassName('foodList')
      let height=0;
      for(let i=0;i<foodList.length;i++){
        height+=foodList[i].clientHeight
        this.listHeight.push(height)
      }
    },
    selectMenu(index,event){
      let foodList=this.$refs.foodWrapper.getElementsByClassName('foodList')
      this.foodScroll.scrollToElement(foodList[index],300)
    }
  },
  mounted(){
    this.$nextTick(()=>{
      this.initScroll()
      this.calculateHeight()
    })
  },
  computed:{
    currentIndex(){
      for(let i=0;i<this.listHeight.length;i++){
        let height1=this.listHeight[i]
        let height2=this.listHeight[i+1]
        if(!height2 || (this.scrollY>=height1 && this.scrollY<height2)){
          return i;
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .wrapper{
    padding:0 10px;
  }
  .menuWrapper,.foodWrapper{
    position: absolute;
    top:0;
    bottom:50px;
    z-index:1;
    overflow: hidden;
  }
  .menuWrapper{
    left:0;
    width:20%;
    .menuContent{
      li{
        height:200px;
        line-height:200px;
        text-align: center;
        background:rgb(179, 215, 245);
      }
      .menuList.active{
        background:rgb(248, 165, 165);
      }
    }
  }
  .foodWrapper{
    margin-left:10px;
    right:0;
    width:80%;
    .foodContent{
      li{
        &:nth-child(1){
          height:500px;
          background:red;
        }
        &:nth-child(2){
          height:1000px;
          background: yellow;
        }
        &:nth-child(3){
          height:100px;
          background: blueviolet;
        }
        &:nth-child(4){
          height:300px;
          background: pink;
        }
        &:nth-child(5){
          height:1500px;
          background: goldenrod;
        }
      }
    }
  }
</style>
