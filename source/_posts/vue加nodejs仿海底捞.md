---
title: vue加nodejs仿海底捞
date: 2016-04-13 14:55:24
tags: Vue
categories: Vue
---
### 前言

　　最近一段时间一直在学习`Vue2.0`的相关知识，之前只是看过相关的视频教学，但是一直没有动手去实践自己的项目，这次决定用`Vue2.0`去模仿一个移动端`App`,看了很多自己用过的`App`,最后决定模仿做海底捞移动端`App`,写下这篇博客，记录实践中的心得体会

### 项目构建
1.选用`webpack、vue-cli`脚手架来快速搭建我们的项目骨架。
`Vue`创建项目命令：

```
Vue init webpack hdl
vue init webpack-simple hdl  (没有语法检查)
```
- VUE
	- components: 存放项目的主要组件
	- static:存放静态css js 
	- assets存放img
	
- Node
	- model用于存放mongodb和静态地址和mongodb增删改查方法
	- public用于放后台js和css文件
	- router放路由器
	- view后台ejs 文件

![项目目录](http://otue68nu2.bkt.clouddn.com/17-7-29/4331807.jpg)
![node后台目录](http://otue68nu2.bkt.clouddn.com/17-7-29/15339347.jpg)

2.package依赖

```
  "dependencies": {
    "css-loader": "^0.25.0",
    "element-ui": "^1.3.7",
    "file-loader": "^0.9.0",
    "jquery": "^3.2.1",
    "mint-ui": "^2.2.7",
    "style-loader": "^0.18.2",
    "url-loader": "^0.5.9",
    "vue": "^2.3.3",
    "vue-resource": "^1.3.4",
    "vue-router": "^2.7.0",
    "vuex": "^2.3.1"
  }
```
> npm i
###  webpack配置

```
var path = require('path')
var webpack = require('webpack')

module.exports = {
  entry: './src/main.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: 'dist/',
    filename: 'build.js'
  },
  module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
            // Since sass-loader (weirdly) has SCSS as its default parse mode, we map
            // the "scss" and "sass" values for the lang attribute to the right configs here.
            // other preprocessors should work out of the box, no loader config like this necessary.
            'scss': 'vue-style-loader!css-loader!sass-loader',
            'sass': 'vue-style-loader!css-loader!sass-loader?indentedSyntax'
          }
          // other vue-loader options go here
        }
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
      },
       {
        test: /\.json$/,
        loader: 'json-loader'
      },
       {
        test: /\.css$/,
        loader: 'style-loader!css-loader'
      },
      {
        test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
        loader: 'file-loader'
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?\S*)?$/,
        loader: 'file-loader',
        query: {
          name: '[name].[ext]?[hash]'
        }
      },
      {
        test: /.(jpg|png|gif|svg)$/, 
        use: ['url-loader?limit=8192&name=./[name].[ext]']
      },/*解析图片*/
    ]
  },
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      // 'assets': path.resolve(__dirname, '../../src/assets')
      'jquery':'jquery'
    }
 
  },
  plugins: [
      new webpack.ProvidePlugin({
          $: "jquery",
          jQuery: "jquery"
      })
   ],
  devServer: {
    historyApiFallback: true,
    noInfo: true
  },
  performance: {
    hints: false
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false
      }
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true
    })
  ])
}
```
> 注意点

引入jq插件
```
 plugins: [
      new webpack.ProvidePlugin({
          $: "jquery",
          jQuery: "jquery"
      })
   ],
```
这两个loader顺序不能反
```
'style-loader!css-loader'
```
mui-ul必须配置字体loader

```
 {
        test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
        loader: 'file-loader'
      },
```
### main.js配置

```
import Vue from 'vue';
import App from './App.vue';
import ElementUI from 'element-ui';//引入element-ui
import 'element-ui/lib/theme-default/index.css';//引入element.css样式
import Vuex from 'vuex';//引入vuex
import VueResource from 'vue-resource';//引入vueResource
import VueRouter from 'vue-router';//引入vuerouter
import  './components/static/css/reset.css';
import  './components/static/css/common.css';
import  './components/static/css/order.css';
import  './components/static/css/animate.css';
import  './components/static/css/cart.css';
import  './components/static/css/store.css';
import  './components/static/css/storedetail.css';
import  './components/static/css/ticket.css';
import  './components/static/css/adress.css';
import  './components/static/css/user.css';
import MintUI from 'mint-ui'
import 'mint-ui/lib/style.css'
import $ from 'jquery';
import { InfiniteScroll } from 'mint-ui';

    Vue.use(InfiniteScroll);
Vue.use(VueRouter)//使用vuerouter
Vue.use(Vuex)//使用vux
Vue.use(ElementUI)//使用elementUi
Vue.use(VueResource)//使用vueresonrce
//创建组件引入组件
import Home from './components/home.vue';
import Goods from './components/goods.vue';
  import Store from './components/store.vue';
  import StoreDetail from './components/storedetail.vue';
  import Ticket from './components/ticket.vue';
    import Outsend from './components/order.vue';
    import User from './components/user.vue';
//配置路由
const routes=[
  {path:"/home",component: Home},
  {path:"/",component: Home},
  {path:"/goods",component: Goods},
  {path:"/store",component: Store},
  {path:"/storeDetail",component: StoreDetail},
  {path:"/ticket",component: Ticket},
  {path:"/order",component: Outsend},
  {path:"/user",component: User},
  {path:"*",redirect: Home}//重定向
]
//实例化vueRouter
const router= new VueRouter({
    routes//（缩写）相当于 routes: routes
})
//挂载到vue的实例上
new Vue({
  router, 
  el: '#app',
  render: h => h(App)
})
```
路由器配置好以后需要在`app.js`的`template加上<router-view></router-view>`
### 主页
![](http://otue68nu2.bkt.clouddn.com/17-7-29/61607769.jpg)
flex布局
### 商品页面
![](http://otue68nu2.bkt.clouddn.com/17-7-29/56078577.jpg)
弹出框用的mint-ui的messageBOX

![数据结构](http://otue68nu2.bkt.clouddn.com/17-7-29/67072615.jpg)
数据是从后台数据库vue-resource请求api接口拿到数据
![](http://otue68nu2.bkt.clouddn.com/17-7-29/65069605.jpg)
- 加入购物车小球抛物线效果，使用css3动画大盒子嵌套小盒子，小盒子向左移动，父盒子向下移动实现的抛物线效果;

```
var x=e.clientX;
                var y=e.clientY;
                var difX=x;   /*获取小球距离左侧的距离*/
                var difY=document.documentElement.clientHeight-y+20;
                var outer=document.createElement("div");//创建父盒子
                outer.className="out";
                outer.style.top=(y-20)+"px";
                outer.style.left=(x)+"px";//设置父盒子的位置
                var inner=document.createElement("div");//创建小盒子
                inner.className="addcart";
                outer.appendChild(inner);
                document.body.appendChild(outer);
                var timer=setTimeout(function(){
                    outer.style.transform="translate(0,"+difY+"px)";
                     //里层设置小黑子的translate
                         inner.style.transform = 'translate(-'+difX+'px,0) rotate(720deg)';
                         inner.style.zIndex='100'

                         clearTimeout(timer);//清除定时器
                });
                var deleteTimer=setTimeout(function(){
                    document.body.removeChild(outer)
                },800);
                this.count=this.count+1;
```
### 购物车
![](http://otue68nu2.bkt.clouddn.com/17-7-29/25188940.jpg)
用的是mint-ui的cart，结算点击以后会存入`sessionStorage`，跳转至付款页面，`this.$router.go(-1)`返回时在判断`sessionStorage`是否有值，有的话继续渲染到购物车里面，没有的话清空购物车；
### 用户填写地址页面
![外送](http://otue68nu2.bkt.clouddn.com/17-7-29/75779537.jpg)
- 从sessionStorage里面获取到数据
- 点击结算按钮是必须先填写地址和选择用餐人数不然会触发toast
- 信息填写完毕点击购买以后数据会传入mongodb用户数据表
![](http://otue68nu2.bkt.clouddn.com/17-7-29/97703095.jpg)
外送地址添加界面
- 点击触发计数器，请求api接口服务器session保存用户名和验证码
- 点击确定在请求api接口，服务器接收用户输入验证码，
- 服务器从session获取用户信息和验证码，验证用户验证码是否正确在返回status
![用餐人数选择弹窗框](http://otue68nu2.bkt.clouddn.com/17-7-29/45729789.jpg)
用餐人数选择弹窗框，使用mint-ui的Picker和Button
![](http://otue68nu2.bkt.clouddn.com/17-7-29/90034654.jpg)
自取选择弹窗框，使用mint-ui的Picker和Button，数据是从请求后台的api接口获取
![](http://otue68nu2.bkt.clouddn.com/17-7-29/17880590.jpg)
自取界面
- 信息填写完毕以后会请求api接口加入用户后台订单数据库，状态为自取状态
- ![](http://otue68nu2.bkt.clouddn.com/17-7-29/97703095.jpg)
- 添加验证码请求接口，防止用户频繁请求

###  门店订餐页面
![](http://otue68nu2.bkt.clouddn.com/17-7-29/39222675.jpg)
通过从后台请求接口返回分页数据，使用下拉更新；
![](http://otue68nu2.bkt.clouddn.com/17-7-29/55053228.jpg)
店铺详情页
- `<router-link :to="{ path: '/storeDetail', query: { id: item._id}}">`路由跳转传ip
- `var id=this.$route.query.id;`获取id请求api接口获取数据渲染页面
- 轮播图mint-ui swiper

![](http://otue68nu2.bkt.clouddn.com/17-7-29/22983718.jpg)
![](http://otue68nu2.bkt.clouddn.com/17-7-29/25789466.jpg)
订座页面确认以后会请求api接口把用户信息导入order表
###  小结

子组件在props中创建一个属性，用来接受父组件传过来的数据。
父组件中注册和引用子组件。
在子组件变迁中添加子组件props中创建的属性。
点击事件触发子组件show()方法时将数据一并赋值给改属性。
父组件可以调用子组件中的方法。
###  Vue中transition过渡动画

App很多地方会有点击后出现一个新的页面的操作，如果不做动画看起来会感觉差点什么，Vue中也提供了了过渡动画的API，官网教程：https://cn.vuejs.org/v2/guide/transitions.html。