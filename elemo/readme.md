## 开发整个项目时  首先搭建脚手架工具
> 参考文档搭建脚手架(https://cli.vuejs.org/zh/)  毕竟这个视频里的Vue是1.0版本的
> 要配合更新   使用Vue2.x的版本开发

##  模拟mack数据
> 如果后端接口尚未开发完成，前端开发一般使用mock数据。
新版的vue-cli 自动搭建的build 文件里没有dev-server.js 和 dev-client.js ，因此我们要在webpack.dev.conf.js 里配置

1)复制data.json文件到src/staict/目录下

2)找到bulid目录下 webpack.dev.conf.js 找到 const portfinder = require('portfinder')，在其下添加mock 数据
```
// 添加mock 数据
const express = require('express')
const app = express()
//这里的路径一定要写对  我是直接放在static目录下的  你可以直接放在src目录下（./data.json）
var appData = require('../static/data.json') //加载本地数据文件  
var seller=appData.seller;
var goods=appData.goods;
var ratings=appData.ratings;
var apiRoutes = express.Router()
app.use('/api', apiRoutes)
```
3)接着找到 devServer 里的 watchOptions，其后紧跟
```
watchOptions: {
      poll: config.dev.poll,
    },
    before(app) {
      app.get('/api/seller', (req, res) => {
        res.json({
          errno: 0,
          data: seller
        })//接口返回json数据，上面配置的数据appData就赋值给data请求后调用
      }),
      app.get('/api/goods', (req, res) => {
        res.json({
          errno: 0,
          data: goods
        })
      }),
      app.get('/api/ratings', (req, res) => {
        res.json({
          errno: 0,
          data: ratings
        })
      })
    }
```
4)设置保存后 npm run dev 运行 访问http://localhost:8080/api/seller 就可接收到 该路由对应的json 数据

###  关于自定义路由的加载vue-router
> Vue1.0和Vue2.x的区别还是蛮大的  Vue2.x做的改变更方便我们实现路由的加载
我是直接在components里创建三个组件，然后在router/index.js里加载组件
```
import goods from 'components/goods/goods'
import seller from 'components/seller/seller'
import ratings from 'components/ratings/ratings'

Vue.use(Router)

export default new Router({
    routes:[
        {
            path:'/',
            redirect:'/goods' //默认其实页面
        },
        {
            path:'/goods',
            component:goods
        },
        {
            path:'/seller',
            component:seller
        },
        {
            path:'/ratings',
            component:ratings
        }
    ]
})
```
然后定义一个tab组件  在App.vue里之间导入组件   在tab组件里引入之前创建三个组件
```
<div class="tab border-1px">
        <router-link tag="a" to="/goods" class="tab-item">商品</router-link>
        <router-link tag="a" to="/ratings" class="tab-item">评价</router-link>
        <router-link tag="a" to="/seller" class="tab-item">商家</router-link>
</div>
```
### axios 请求数据
（1）安装 axios
 > npm install axios
（2）引入axios组件
> import axios from 'axios'
（3）axios 请求数据（在此之前创建一个接受数据的对sellerobj）
```
export default {
 //  获取数据 准备  返回一个对象，后台获取数据后 赋予 该对象
   data (){
   	 return {
   	 	 seller:{}
   	 }
   },
   created (){ //  创建之前 请求数据
   	 axios.get('static/data.json').then((result) => {
   	  	 console.log(result) //  控制台检查  数据存储在  result.data 里  
   	  	this.sellerobj = result.data.seller //  将数据存到sellerobj里  	 	
   	  })
   }
}
```
        项目的初始化工作就差不多完成了
