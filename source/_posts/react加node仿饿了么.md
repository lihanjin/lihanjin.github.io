---
title: react加node仿饿了么
date: 2016-01-13 14:55:24
tags: React
categories: React
---
### 前言

　　最近一段时间一直在学习`React`的相关知识，，这次决定用`React`去模仿一个移动端`App`,看了很多自己用过的`App`,最后决定模仿做饿了么移动端`App`,写下这篇博客，记录实践中的心得体会
### 项目构建
基于`react、react-redux、react-router、antd、mongodb`实现单个商家页部分，后台数据通过`nodejs`模拟实现。
1.安装脚手架工具   （单文件组件项目生成工具）   只需要安装一次



```
npm install -g create-react-app   /  cnpm install -g create-react-app
```

2.创建项目   （可能创建多次）
	

	找到项目要创建的目录：

	create-react-app reactdemo



3.cd  到项目里面	
	
	cd  reactdemo


	npm start  运行项目


	npm run build 生成项目

### 项目目录文件
使用插件

1、react（组件编写）

2、react-router（控制路由3.x）

3、react-dom（渲染dom）

4、redux（控制状态）

5、react-redux（更新组件状态）

6、react-promise（异步处理）

7、react-tap-event-plugin（手机tap事件）

8、ant-design-mobile（移动端ui组件）

9、better-scroll（页面滚动）
1.package.json

```
 "dependencies": {
    "antd": "^2.11.2",
    "antd-mobile": "^1.4.1",
    "axios": "^0.16.2",
    "md5": "^2.2.1",
    "react": "^15.6.1",
    "react-dom": "^15.6.1",
    "react-router-scroll": "^0.4.2",
    "touchjs": "^0.2.14"
  },
```
2.入口app.js

```
import React, { Component } from 'react';
import './component/static/css/antd.css';
import "./component/static/css/reset.css";
import "./component/static/css/common.css";
import $ from "jquery";
import Footer from "./component/footer";
import Order from './component/order';
import User from './component/user';
import Find from './component/find';
import Home from "./component/home";
class App extends Component {
  render() {
    return (
      <div className="App">
        
       
        {this.props.children}
        
      </div>
    );
  }
}

export default App;
```
3.index.js路由配置界面

```
import React from 'react';
import ReactDOM from 'react-dom';
import "./component/static/css/index.css";
import App from './App';
import registerServiceWorker from './registerServiceWorker';
import {Router,Route,Link,IndexRoute,browserHistory} from 'react-router'
import Order from './component/order';
import User from './component/user';
import Collection from './component/user/collection';
import Login from './component/user/Login';
import Reg from "./component/user/Reg";
import Reg1 from "./component/user/Reg1";
import Reg2 from "./component/user/Reg2";
import Find from './component/find';
import Search from './component/search';
import Home from "./component/home";
import Seller from "./component/home/seller";
import Detail from "./component/detail";
import Collect from "./component/collect";
ReactDOM.render(
<Router history={browserHistory}>
    <Route path="/" component={App}>
        <IndexRoute component={Home}/>
        <Route path="home" component={Home}>

        </Route>
        <Route path="find" component={Find}>

        </Route>
        <Route path="order" component={Order}>

        </Route>
        <Route path="user" component={User}>

        </Route>
        <Route path="user/collection" component={Collection}></Route>
        <Route path="user/login" component={Login}></Route>
        <Route path="login" component={Login}></Route>
          <Route path="reg" component={Reg}></Route>
        <Route path="user/reg" component={Reg}></Route>
        <Route path="reg1" component={Reg1}></Route>
        <Route path="user/reg1" component={Reg1}></Route>
        <Route path="reg2" component={Reg2}></Route>
        <Route path="user/reg2" component={Reg2}></Route>
        <Route path="user/collect" component={Collect}></Route>
        <Route path="/quite" component={User}></Route>
          <Route path="seller/:aid" component={Seller}></Route>
        {/*<Route path="seller" component={Seller}></Route>*/}
           <Route path="search" component={Search}></Route>
        <Route path="detail" component={Detail}></Route>
    </Route>
</Router>, 
document.getElementById('root')
);
registerServiceWorker();
```
`browserHistory`去路径的#号
`IndexRoute`设置默认路由
4.storage.js

```
var app={

    set:function(key,value){
        localStorage.setItem(key,JSON.stringify(value));
    },
    get:function(key){
        return JSON.parse(localStorage.getItem(key));

    },
    hasCollectionData(aid,collectdata){  /*判断collectdata里面有没有数据*/


            //forEach是个异步

        for(var i=0;i<collectdata.length;i++){

            if(aid==collectdata[i].aid){

                return true;
            }
        }

        return false;


    }
}

export default  app;
```

`localstorage`方法封装
### 项目详情
#### 主页
![](http://otue68nu2.bkt.clouddn.com/17-7-31/35682381.jpg)
使用Swiper实现轮播图效果
flex布局
####  商家列表
![](http://otue68nu2.bkt.clouddn.com/17-7-31/96299879.jpg)
商家列表通过下拉更新请求pai接口从数据库获取模拟数据渲染页面
#### 搜索页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/54613544.jpg)
历史搜索使用localstorage保存本地进行渲染

```
router.post("/search",function(req,res,next){

    var keyword=req.body.data
   DB.Find("trade",{$or:[{"title":{ $regex:new RegExp(keyword)}},{"name":{ $regex:new RegExp(keyword)}}]},function(err,data){

       if(data.length>0){
           res.json(data)
       }else {
           res.json(404)
       }

   })

})
```
服务端通过nodejs的模糊匹配搜索数据库符合的商家进行调整至商家详情页面
#### 用户个人中心
![](http://otue68nu2.bkt.clouddn.com/17-7-31/92226705.jpg)
#### 用户注册页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/52113968.jpg)
1.通过验证码点击事件判断，获取用户输入的账号，查询数据库是否已经存在，如果不存在会返回验证码信息
2.通过确认点击事件，获取用户输入的账号信息和验证码是否正确，如果正确保存在数据库，提示注册成功。

```
router.post('/doReg1', function(req, res, next) {
    Rphone = req.body.getphone;
    var longstr = '1,2,3,4,5,6,7,8,9,0,A,B,C,D,E,F,G,H,I,G,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z';
    var str = longstr.split(',');
    var prevNum = '';
    for(var i = 0; i < 6; i++) {
        prevNum += str[Math.floor(Math.random() * 36)];
    }
    DB.Find("user",{phone},function(err,data){
        if(err){
            console('error');
        }else {
            phone=Rphone
            if(data==""){
                res.json({'msg':"手机号可用","status":"1","prevNum":prevNum})
            }else{
                res.json({'msg':"手机号已被注册","status":"0"})
            }
        }
    })

})
```
#### 用户登录页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/34231641.jpg)
#### 用户订单页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/74801433.jpg)
用户已经登录未购买商品页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/12618238.jpg)
用户未登录页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/82983607.jpg)
用户登录并且购买过商品页面
通过判断localstorage的是否存在来判断用户是否登录，如果登录通过localstorage的信息查询数据来改变state的状态并且显示不同的页面。
#### loading加载页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/49835193.jpg)

```
.load-top{
background: url("../img/loading.png") no-repeat;
width:.5rem;
height:.5rem;
background-size: 100% auto;
position:absolute;
background-size: 100% auto;
transform-origin: 50% 50%;
animation: change 3.6s infinite steps(6),trans .3s ease-in-out infinite alternate;
}
.w12{
width:.22rem;
border-radius: 30%;
}
.load-bot{
background: url("../img/load.png") no-repeat;
top: 50%;
width:.5rem;
height:.5rem;
background-size: 100% auto;
position:absolute;
background-size: 100% auto;
transform-origin: 50% 50%;
background-position-y: .2rem;
animation: scale .3s ease-in-out infinite alternate;
}
@keyframes scale{
100%{
transform: scale(.3);
}
}
@keyframes change{
100%{
background-position: 0 -3rem;
}
}
@keyframes trans{
100%{
transform: translateY(-2.5em)
}
```
![](http://otue68nu2.bkt.clouddn.com/17-7-31/82778838.jpg)
通过css3动画实现
#### 商家商品详情页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/94716560.jpg)
使用fly实现购物车飞入效果，右上角为用户收藏事件，通过storage拿到用户信息查询数据库搜索用户收藏列表返回状态改变，用户点击以后再更改用户收藏店铺表的状态。
![](http://otue68nu2.bkt.clouddn.com/17-7-31/61232016.jpg)
![](http://otue68nu2.bkt.clouddn.com/17-7-31/7739692.jpg)
购物车和付款页面，通过redux实现用户跳转订单页面没有购买调回，用户购物车信息不丢失，如果用户付款成功清空rendux的购物车，并且跳转至订单页面；
![](http://otue68nu2.bkt.clouddn.com/17-7-31/36502811.jpg)
订单页面
![](http://otue68nu2.bkt.clouddn.com/17-7-31/14750992.jpg)
用户收藏页面

