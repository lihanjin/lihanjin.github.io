---
title: Nodejs的async
date: 2016-01-20 09:35:08
tags: 
    - NodeJS
categories: NodeJS
---
### async模块是为了解决嵌套金字塔,和异步流程控制而生.常用的方法介绍
1.安装  `npm install async --save`
2.引入  `var async = require('async')`

```
var async = require('async')
```



//series: 串行执行，一个函数数组中的每个函数，每一个函数执行完成之后才能执行下一个函数。

//parallel：并行(parallel)  并行无关联   一个函数数组中的每个函数同步执行。

### 没有使用async的串行查询数据库

```
DB.find('user',function(){


   DB.find('admin',function(){

       DB.find('admin',function(){

           DB.find('admin',function(){

               res.render('index'，{


               })
           })
       })
   })

})
```
### 使用async的串行查询数据库
### 串行series
1.数组方式

console.time()
console.timeEnd()//打印执行时间
```
console.time('test');

async.series([
   function(callback){  /*异步*/

       setTimeout(function(){
           callback(null,'one')

       },2000)

   },
   function(callback){  /*异步*/

       setTimeout(function(){

           callback(null,'two')
       },3000)

   }
   ,
   function(callback){  /*异步*/

       setTimeout(function(){

           callback(null,'three')
       },1000)

   }


],function(err,data){


   console.timeEnd('test')
   console.log(data);
})
```
2.对象方式


```
console.time('list');
async.series({

   list:function(callback){

       //新闻列表

       setTimeout(function(){

           callback(null,'新闻列表')
       },1000)



   },
   cate:function(callback){
       //分类

       setTimeout(function(){

           callback(null,'新闻分类')
       },1000)

   }

},function(err,data){
   console.timeEnd('list');
   console.log(data);

})
```
### 并行 paraller

```
//并行

console.time('list');
async.parallel({
    list:function(callback){

        //新闻列表
        setTimeout(function(){

            callback(null,'新闻列表')
        },3000)
    },
    cate:function(callback){
        //分类

        setTimeout(function(){

            callback(null,'新闻分类')
        },2000)

    }

},function(err,data){
    console.timeEnd('list');
    console.log(data);

})
```



