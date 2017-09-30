---
title: Angular4基础总结
date: 2017-04-13 14:55:24
tags: Angular
categories: Angular4.x
---
### 一、文档
官方文档https://angular.cn/docs/ts/latest/quickstart.html
知乎angular简单入门笔记https://zhuanlan.zhihu.com/p/27696268
邹业盛 angular的学习笔记http://zouyesheng.com/angular.html
Angular.js 的一些学习资源https://github.com/dolymood/AngularLearning

----------

### 二、安装
- 1.安装最新版本的nodejs
注意：请先在终端/控制台窗口中运行命令 node -v 和 npm -v， 来验证一下你正在运行 node 6.9.x 和 npm 3.x.x 以上的版本。 更老的版本可能会出现错误，更新的版本则没问题。
- 2.全局安装 Angular CLI 脚手架工具

  - 使用 npm命令安装

```
npm install -g @angular/cli
```
```
cnpm install -g @angular/cli
```
----------

### 三、创建项目
1.	打开cmd找到你要创建项目的目录
2.	创建项目

	`ng new` 项目名称  创建一个项目
	
	```
	ng new my-app
	```
	
3.	进入刚才创建的项目里面启动服务
	```
	cd my-app
	cnpm install 
	ng serve --open
	```
----------

### 四、目录结构分析
https://angular.cn/docs/ts/latest/cli-quickstart.html
- src文件夹
你的应用代码位于src文件夹中。 所有的Angular组件、模板、样式、图片以及你的应用所需的任何东西都在那里。 这个文件夹之外的文件都是为构建应用提供支持用的。

### 五、创建angualr组件 使用组件
Component

```
ng g component my-new-component   
```

指定目录创建 ：`ng g component components/Footer`
Directive

```
ng g directive my-new-directive
```

Pipe

```
ng g pipe my-new-pipe
```

Service

```
ng g service my-new-service
```

Class

```
ng g class my-new-class
```

Guard

```
ng g guard my-new-guard
```

Interface

```
ng g interface my-new-interface
```


Enum
```
ng g enum my-new-enum
Module
```

```
ng g module my-module
```

- 1.`app.components.ts`

```
	 import { Component } from '@angular/core';   /*引入angular核心*/
	
	@Component({  
	  selector: 'app-root',  /*标签  html里面用的组件名称*/
	  templateUrl: './app.component.html',  /*模板*/
	  styleUrls: ['./app.component.css']  /*css样式*/
	})
	export class AppComponent {
	  title = 'app';  /*属性  类*/
	
	
	  msg="你好angular4.x"
	
	
	}
```
- 2.`app.module.ts`

```
// 定义AppModule，这个根模块会告诉Angular如何组装该应用,组件 服务得在这个配置

import { BrowserModule } from '@angular/platform-browser';  /*浏览器相关的*/
import { NgModule } from '@angular/core';  /*核心模块*/

//要用 双向数据绑定必须引入 FormsModule
import { FormsModule } from '@angular/forms';  



//使用http请求数据需要首先引入


import { HttpModule, JsonpModule } from '@angular/http';



 
import { AppComponent } from './app.component';
import { HomeComponent } from './components/home/home.component';
import { NewsComponent } from './components/news/news.component';
import { TodolistComponent } from './components/todolist/todolist.component';  /*根组件*/
//引入服务



import { StorageService } from './services/storage.service';
import { NewslistComponent } from './components/newslist/newslist.component';  /*不需要加ts*/




@NgModule({
  declarations: [     /*申明组件   依赖注入*/
    AppComponent, HomeComponent, NewsComponent, TodolistComponent, NewslistComponent
  ],
  imports: [   /*模块都放在这个里面*/
    BrowserModule,
    FormsModule,
    HttpModule,   /*注入模块*/
    JsonpModule
  ],
  providers: [StorageService],  /*所有的服务*/
  bootstrap: [AppComponent]   /*默认启动的组件*/
})
export class AppModule { }   /*暴露模块 默认就行*/
	
```

1.数据文本绑定

```
<h1>
{{title}}
</h1>
```
2.绑定html
```
this.h="<h2>这是一个h2用[innerHTML]来解析</h2>"
<div [innerHTML]="2"></div>
```
3`*ngFor`普通循环

```
 <ul>
    <li *ngFor="let item of list">
      {{item}}
    </li>
  </ul>
```

```
//带index下标的循环
<ul>
    <li *ngFor="let item of list2; let i = index">
       {{i}}----- {{item.title}}
    </li>
</ul>
```
4、条件判断


```
  <p *ngIf="list.length > 3">这是ngIF判断是否显示</p>
```


5、执行事件

```
<button class="button" (click)="getData()">
      点击按钮触发事件
  </button>

  getData(){ /*自定义方法获取数据*/
    //获取
    alert(this.msg);

  }
```
6.双向数据绑定
`<input [{ngModel}]="inputValue>"`
**注意引入FormsModule**

```
import { FormsModule } from '@angular/forms'; 
```

```
@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    FooterComponent,
    NewsComponent
  ],
  imports: [
    BrowserModule,
    FormsModule//这里引入
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
使用：

```
  <input type="text"  [(ngModel)]="inputValue"/>

  {{inputValue}}
```
### 六、创建angualr服务使用服务

```
ng g service my-new-service

创建到指定目录下面
ng g service services/storage
```
1.`app.module.ts`里面引入创建的服务
```
import {StorageService} from "./services/storage.service"
```
2.`NgModule`里面的`providers`里面依赖注入服务

```
@NgModule({
  declarations: [
    AppComponent,
    HeaderComponent,
    FooterComponent,
    NewsComponent,
    TodolistComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [StorageService],//这里注入
  bootstrap: [AppComponent]
})
export class AppModule { }
```
3.使用页面引入服务，注册服务

```
import { StorageService } from '../../services/storage.service';
```

```
constructor(private storage: StorageService) {
	   
}
```


使用

```
addData(){

      // alert(this.username);

      this.list.push(this.username);

      this.storage.set('todolist',this.list);
  }
  removerData(key){
    
      console.log(key);
      this.list.splice(key,1);
      this.storage.set('todolist',this.list);

  }
```
### 七、Angular4.x  get post以及 jsonp请求数据

不使用`rxjs`请求数据


1.引入`HttpModule 、JsonpModule`

普通的 `HTTP` 调用并不需要用到 `JsonpModule`，不过稍后我们就会演示对 `JSONP` 的支持， 所以现在就加载它，免得再回来改浪费时间。



```
import { HttpModule, JsonpModule } from '@angular/http';
```

2.`HttpModule 、JsonpModule`依赖注入



```
@NgModule({
  declarations: [
    AppComponent,
    HomeComponent,
    NewsComponent,
    NewscontentComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    JsonpModule,
    AppRoutingModule
  ],
  providers: [StorageService,NewsService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

使用`Http、Jsonp`：
1、在需要请求数据的地方引入 `Http`

```
import {Http,Jsonp} from "@angular/http";
```

2、构造函数内申明：

```
constructor(private http:Http,private jsonp:Jsonp) { }
```


3、对应的方法内使用http请求数据

   

```
this.http.get("http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1")
      .subscribe(
         function(data){
           console.log(data);
         },function(err){
          console.log('失败');
         }
      );
```

   

```
this.jsonp.get("http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1&callback=JSONP_CALLBACK")
      .subscribe(
         function(data){
           console.log(data);
         },function(err){
          console.log('失败');
         }
      );
```

使用`Post`

1.	引入`Headers 、Http`模块  用的地方


```
import {Http,Jsonp,Headers} from "@angular/http";
```


2.	实例化`Headers`


  

```
private headers = new Headers({'Content-Type': 'application/json'});
```


3.`post`提交数据

```
	this.http
	    .post('http://localhost:8008/api/test', 
	    JSON.stringify({username: 'admin'}), {headers:this.headers})
	    // .toPromise()
	    .subscribe(function(res){
	      console.log(res.json());
	    });
```
    
  
  使用rxjs请求数据

RxJS是一种针对异步数据流编程工具，或者叫响应式扩展编程；可不管如何解释RxJS其目标就是异步编程，Angular引入RxJS为了就是让异步可控、更简单。

大部分RxJS操作符都不包括在Angular的Observable基本实现中，基本实现只包括Angular本身所需的功能。
如果想要更多的RxJS功能，我们必须导入其所定义的库来扩展Observable对象， 以下是这个模块所需导入的所有RxJS操作符：

1、	引入`Http 、Jsonp、RxJs`  模块

```
import {Http,Jsonp} from "@angular/http";

import {Observable} from "rxjs";
import "rxjs/Rx";
```


你可能并不熟悉这种import 'rxjs/Rx'语法，它缺少了花括号中的导入列表：{...}。
这是因为我们并不需要操作符本身，这种情况下，我们所做的其实是导入这个库，加载并运行其中的脚本， 它会把操作符添加到Observable类中。

2、	构造函数内申明：

```
constructor(private http:Http,private jsonp:Jsonp) { }
```

3、get请求

```
this.http.get("http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1")
      .map(res => res.json()) .subscribe(
         function(data){
           console.log(data);
         }
      );
```
4、Jsonp请求

    

this.jsonp.get("http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1&callback=JSONP_CALLBACK")
  .map(res => res.json()) .subscribe(
     function(data){
       console.log(data);
     }
  );

`http.get` 方法中返回一个`Observable`对象，我们之后调用RxJS的map操作符对返回的数据做处理。


### 八、路由

`ng new` 项目名称  `--routing`


```
ng new angulardemo02 –routing
```


```
app-routing.module.ts

const routes: Routes = [
  {
    path: 'home',
    component:HomeComponent
    // children: []
  },
   {
    path: 'news',
    component:NewsComponent
    // children: []
  }
   { path: '',   redirectTo: '/home', pathMatch: 'full' },
];
```

动态路由

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { NewsComponent } from './components/news/news.component';
import { HomeComponent } from './components/home/home.component';

import { NewcontentComponent } from './components/newcontent/newcontent.component';

const routes: Routes = [
  {
    path: 'home',
    component:HomeComponent
    // children: []
  },
   {
    path: 'news',
    component:NewsComponent
    // children: []
  },{
    path: 'newscontent/:aid',
    component:NewcontentComponent
    // children: []
  },
   { path: '',   redirectTo: '/home', pathMatch: 'full' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

获取传值
1.引入

```
import {ActivatedRoute } from '@angular/router';
```

2.实例化

```
constructor(private route:ActivatedRoute) { }
```

3.

```
ngOnInit() {

    // console.log();
    this.route.params.subscribe(function(data){
      console.log(data);
    })
  }
```
九、父子组件传值

1.	父组件给子组件传值

1.子组件

```
import { Component, OnInit ,Input } from '@angular/core';
```

2.父组件调用子组件

```
<app-header [msg]="msg"></app-header>
```


3.子组件中接收数据

```
export class HeaderComponent implements OnInit {
  
  @Input() msg:string

  constructor() { }

  ngOnInit() {

  }

}
```





2.	子组件给父组件传值

1.	子组件引入`Output 和 EventEmitter`


```
import { Component, OnInit ,Input,Output,EventEmitter} from '@angular/core';
```

2.子组件中实例化 `EventEmitter`

  

```
@Output() private outer=new EventEmitter<string>();  
```

 

```
 
  /*用EventEmitter 和output装饰器配合使用  <string>指定类型变量*/
```


3. 子组件通过  `EventEmitter` 对象`outer`实例广播数据


 

```
 sendParent(){
    // alert('zhixing');
    this.outer.emit('msg from child')
  }
```


4.父组件调用子组件的时候，定义接收事件 , `outer`就是子组件的`EventEmitter` 对象`outer`


```
<app-header (outer)="runParent($event)"></app-header>
```



5.父组件接收到数据会调用自己的`runParent`方法，这个时候就能拿到子组件的数据

 
 //接收子组件传递过来的数据

```
  runParent(msg:string){
    alert(msg);  
  }
```
