# 一、安装better-scroll

  cnpm install better-scroll -D
  
# 二、导入

  import BScroll from 'better-scroll'
  
# 三、代码解析

	1、 BScroll初始化
	
	```
	new BScroll('滚动的容器', {
		click:true, // 是否可点击
		probeType:3 // BScroll滚动时，获取实时位置，类似探针效果
	})
	```
	
	2、 $nextTick方法DOM更新执行回调，所以要写在created/mounted钩子函数中
	
	```
	this.$nextTick(()=>{
	  this.initScroll()
	  this.calculateHeight()
	})
	```
	
	3、 统计滚动的高度区间
	
	```
	calculateHeight(){
	  let foodList=this.$refs.foodWrapper.getElementsByClassName('foodList')
	  let height=0;
	  for(let i=0;i<foodList.length;i++){
		height+=foodList[i].clientHeight
		this.listHeight.push(height) 
	  }
	}
	```
	
	4、 获取滚动菜单的索引值，给菜单加上active样式
	
	```
	currentIndex(){
	  for(let i=0;i<this.listHeight.length;i++){
		let height1=this.listHeight[i]
		let height2=this.listHeight[i+1]
		if(!height2 || (this.scrollY>=height1 && this.scrollY<height2)){
		  return i;
		}
	  }
	}
	```
	
	```
	<ul class="menuContent">
		<li class="menuList" :class="{active:currentIndex===index}" v-for="(menu,index) in menus" :key="menu.id" @click="selectMenu(index,$event)">{{menu.title}}</li>
	  </ul>
	```
	
	5、 点击左侧菜单展示对应的内容区
	
	```
	selectMenu(index,event){
	  let foodList=this.$refs.foodWrapper.getElementsByClassName('foodList')
	  this.foodScroll.scrollToElement(foodList[index],300)
	}
	```
