---
title: Angular基础总结
date: 2017-04-13 17:55:24
tags: Angular
categories: Angular
---
## Angular基础总结
### 一、使用Angular
    1.引入`angualrjs`
    2.定义`ng-app`告诉`angualrjs`那些代码要被`angualrjs`解析
    3. `ng-model`  表示把`input`输入的值赋值给 `username`
    4.`{{}}`  绑定数据
         
        
### 二、控制器
- 1.通过`ng-app`定义myApp模块  告诉`angularjs`这里面的代码要被`angularjs`解析
- 2.得通过`angular.module('myApp',[])`  实现
    两个参数: 
    - 1.模块名称    
    - 2.依赖注入的让他模块
           
 4.定义控制器    `view   ng-controller`
 5.定义控制器以后要实现这个控制器
  

```
app.controller('firstController',function($scope){

                 $scope.name='张三';

         })
```

`$scope`视图和控制器的桥梁，所有的数据都要绑定到`$scope` 所有的方法也要绑定到`$scope`

### 三、view和控制器的桥梁
1.`$rootScope`  全局作用域  可以数据和方法  （多个控制器共享数据）

 2.`$scope`   局部作用域
 
### 四、$apply
`AngularJS`外部的控制器（`DOM`事件、外部的回调函数如`jQuery` 事件等）调用了`AngularJS`函数之后，必须调用`$apply`。
     在这种情况下，你需要命令`AngularJS`刷新自已（模型、视图等），`$apply`就是用来做这件事情的。
     `angularjs`外部的事件 或者方法 改变`$scope`绑定的数据的时候，改变不了，这时候就要用到`$apply`
### 五、内置定时器
- 1.`$timeout`
- 2.`$interval`
### 六、数据
- 1.数据绑定
	- `ng-model='name'`,`{{name}}`,可以将`$scope`的数据绑定到视图
- 2.`ng-reapet`循环(`track by $index`出现相同值的时候需要绑定index)
- 3.`ng-bind-html`，数据绑定 `<element ng-bind-html="expression"></element>`
- 4.数字绑定`ng-init`,不常见建议用`ng-bind`

    ```
    <div ng-app="" ng-init="quantity=1;cost=5">
    <p>总价： {{ quantity * cost }}</p>
    </div>
    ```
- 5.`ng-bind`
### 七、`$watch`
`$watch`用于监听每个`scope`中的变量
- 1.`$watch`单一的变量

	```
	$scope.count=1;
	$scope.$watch('count',function(){
	...
	});
	```

- 2.`$watch`多个变量用`+`号隔开

	```
	//当count或page变化时，都会执行这个匿名函数
	$scope.count=1;
	$scope.page=1;
	$scope.$watch('count + page',function(){
	...
	});
	```
- 3.`$watch`页可以监听数组或者对象

	```
	$scope.items=[
	{a:1},
	{a:2}
	{a:3}];
	$scope.$watch('items',function(){...},true);
	```
### 八、`$http`请求
1.`get`请求(`post`同理)
```
var url="http://www.phonegap100.com/appapi.php";
            $http.get(url,{
                params:{
                    'a':'getPortalList',
                    'catid':20,
                    "x":'xxx'
                }
            }).then(function(res){



                console.log(res.data.result);

                $scope.list=res.data.result;



            },function(err){

                console.log(err);
            })
```
2.`jsonp`请求

```
var url='http://www.phonegap100.com/appapi.php?a=getPortalArticle&aid=109&callback=JSON_CALLBACK'

           $http.jsonp(url).success(function(data){
               console.log(data);

           }).error(function(){

           })
```
3.`jsonp`1.6.2的版本

```
var url='http://www.phonegap100.com/appapi.php?a=getPortalArticle&aid=109&callback=JSON_CALLBACK'

           $http.jsonp(url).success(function(data){
               console.log(data);

           }).error(function(){

           })
```

### 九、代码压缩的问题
第二个参数写数组可以完美解决

```
  app.controller('secondController',['$scope','$http',function($scope,$http){

        $scope.msg="你好angualrjs"

//        $http.get()
    }])
```
### 十、模块依赖`ngSanitize.js`（html代码解析模块）
1.引入  `angular-sanitize.js`
2.依赖注入`ngSanitize`
3.`ng-bind-html` 绑定数据

```
  var app=angular.module('myApp',['ngSanitize']);

    app.controller('firstController',function($scope){
        $scope.title='<h2>这是一个标题</h2>'

    })
```
### 十一、`angular`的路由配置

    1.引入js  angular-route.min.js
    2.在html页面中定义  ng-view  显示我们模板里面动态加载的数据
    3.var app=angular.module('myApp',['ngRoute']);   依赖注入
        //根据不同的路由加载不同的控制器和模板

        //加载出来的模板和控制回头放在视图里面
```
 app.config(function($routeProvider){


        $routeProvider.when('/home',{


            templateUrl:"templates/home.html",
            controller:'homeController'

        }).when('/news',{


            templateUrl:"templates/news.html",
            controller:'newsController'
        }).when('/newscontent/:aid',{//动态路由

            templateUrl:"templates/newscontent.html",
            controller:'newsContentController'

        }).otherwise({  /*找不到路由的时候动态跳转的页面*/
            redirectTo:'/home'
        })

    })
```
5.注意：要实现路由里面定义的`controller`
### 十二、`angular`的广播
- `$scope  $rootScope`上面都可以绑定广播

- `$broadcast`  给子`controller` 广播数据
- `$emit`  给 父亲`controller`广播数据
                  
                  
- `$scope.$on  接收数据  ('名称',function(event,data){})`
                  

- `$scope.$broadcast('to-child', '给child数据');`    //前面是名字   后面是数据  数据可以是 字符串  也可以是数组 和对象
- `$scope.$broadcast('to-parent', '给父亲元素广播数据')`
### 十三、`angular`的动画
```
  <script src="http://cdn.bootcss.com/angular.js/1.2.9/angular-animate.min.js"></script>

    <script>

        var m1 = angular.module('myApp',['ngAnimate']);
        m1.controller('firstController',['$scope',function($scope){
            $scope.bBtn = true;
        }]);

        m1.animation('.box',function(){
            return{
                enter:function(element,done){
                    console.log(element);
                    console.log(done);
                    $(element).css({width:0,height:0});

//                 /   $(element).animate({width:0,height:0});
                    $(element).animate({width:200,height:200},1000,done);

                },
                leave:function(element,done){
                    $(element).animate({width:0,height:0},1000,done);

                }

            }

        });

    </script>
</head>

<body>
<div ng-controller="firstController">
    <input type="checkbox" ng-model="bBtn">
    <div ng-if="bBtn" class="box"></div>
</div>
```

       