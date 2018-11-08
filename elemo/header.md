## header组件开发  数据之间的传递
>  App.vue 引入组件，App.vue 中注册组件，使用组件时 要记得传入seller参数
<v-header :sell="sellerObj"></v-header>

通过父组件向子组件传递数据<br>
在父组件中需要将sellerO作为数据导出，子组件通过props从父组件中获得数据，且要指定数据类型
```
export default {
 props:{  //  子组件获取 父组件 数据
 	sell:{
 	  type:Object // 传递的类型 
 	}
  }
 }
``` 
然后在header组件里面调用数据就出来  写入基本的样式

####  header组件的弹出层  
> v-show设置一个状态，判断该状态控制显示隐藏
> @click 绑定点击事件,通过methods 方法改变 状态，控制显隐效果

#####  星级评分
>绑定class 控制星级大小的类型 通过计算属性计算星星大小
```
//  利用 computed 属性
<div class="star" :class="starSizeType"></div>
computed: {
  starSizeType() { //  返回 星级的大小类型  48/36/24
   return 'star-' + this.size;
  }
}
```
> 遍历星星的数量 
 <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>

> 定义常量 控制 每个星的状态
```
// 类名用变量存起来
const LENGTH = 5  //  星星长度
const CLS_ON = 'on'  // 全星
const CLS_HALF = 'half' // 半星
const CLS_OFF = 'off'// 空星
```

> 通过计算 判断每个span 的类型
```
itemClasses () {  //  返回一个数组为每个span 的类名  （遍历）
   let spanClassList=[];
   // 利用 实参评分来判断 有几颗全星，半星，空星
  let scores=( Math.floor(this.score * 2) ) / 2 
  let intNum= Math.floor(scores); // 全星个数  
  let HashalfNum= scores % 1 !== 0   // 半星
  for(var i=0;i<intNum;i++){ // 遍历全星的span
	spanClassList.push(CLS_ON)
  }
  if(HashalfNum){ //  如果有半星  加类名
	spanClassList.push(CLS_HALF)
  }
  while(spanClassList.length<LENGTH){//  判断 是否有空星 及个数
	spanClassList.push(CLS_OFF)
  }
   return spanClassList;	    	
  }
}
```
> 通过 动态绑定class 来 给span 加类名
```
<div class="star" :class="starSizeType">
  <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item" track-by="$index"></span>
</div>
```