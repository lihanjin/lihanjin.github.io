---
title: Angular4.x模拟豆瓣电影
date: 2016-07-13 14:55:24
tags: Angular
categories: Angular
---
### 前言

　　最近一段时间一直在学习`Angular4.x`的相关知识，，这次决定用`Angular4.x`去模仿一个豆瓣电影页面,写下这篇博客，记录实践中的心得体会
### 创建项目


创建项目

ng new 项目名称 创建一个项目

ng new my-app
进入刚才创建的项目里面启动服务

cd my-app
cnpm install 
ng serve --open


----------


创建组件
ng g component my-new-component
指定目录创建 ：ng g component components/Footer
Directive
创建服务
ng g directive my-new-directive
![](http://otue68nu2.bkt.clouddn.com/17-7-31/98583986.jpg)
项目目录结构
#### node后台接口
由于豆瓣电影的api没有jsonp所以通过fs模块的数据流进行模拟数据

```
pp.get('/movie',function(req,res){


	fs.readFile('./cache/movie.txt',function(err,data){

		if(err){
				var https=require('https');
				var url='https://api.douban.com/v2/movie/coming_soon';

				https.get(url, function(response) {
					var data = '';
					response.on('data', function(chunk) {
						data += chunk;
					})
					response.on('end', function() {
						//res.send(data);
						fs.writeFile('movie.txt',data,function(){

						});
						res.jsonp(data);
					})
				});

		}else{

				res.jsonp(data.toString());
		}


	})




})

app.get('/movieinfo',function(req,res){
	//console.log(req.body);
	var id=req.query.id;
	fs.readFile('./cache/movieinfo'+id+'.txt',function(error,data){
			if(error){
				var https=require('https');
				var url='https://api.douban.com/v2/movie/subject/'+id;

				https.get(url, function(response) {
					var data = '';
					response.on('data', function(chunk) {
						data += chunk;
					})
					response.on('end', function() {
						fs.writeFile('./cache/movieinfo'+id+'.txt',data,function(){

						});
						res.jsonp(data);
					})
				});

			}else{
				res.jsonp(data.toString());
			}

	})



})
```
#### app.module.ts配置

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';


import { HttpModule, JsonpModule } from '@angular/http';
//引入http和jsonp的模块


import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { MovieComponent } from './components/movie/movie.component';
import { MovieinfoComponent } from './components/movieinfo/movieinfo.component';

@NgModule({
  declarations: [
    AppComponent,
    MovieComponent,
    MovieinfoComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpModule,
    JsonpModule    
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
#### app.commpont.ts配置

```
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app';
}
```
#### app.routing.module.ts配置
	
ng new angulardemo02 –routing可以直接创建路由很方便

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

import { MovieComponent } from './components/movie/movie.component';
import { MovieinfoComponent } from './components/movieinfo/movieinfo.component';


const routes: Routes = [
  {
    path: 'movie',
    component:MovieComponent
    
  },
  {
    path: 'movieinfo/:id',
    component:MovieinfoComponent    
  },//动态路由

  {
    path: '',
    redirectTo: '/movie',
    pathMatch: 'full'
  }

];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
#### movie代码
1.movie.component.ts

```
import { Component, OnInit } from '@angular/core';

import { Http, Jsonp } from '@angular/http';


@Component({
  selector: 'app-movie',
  templateUrl: './movie.component.html',
  styleUrls: ['./movie.component.css']
})
export class MovieComponent implements OnInit {

  public list:any[];

  public msg:any='这是一个msg';

  constructor(private http:Http,private jsonp:Jsonp) { }

  ngOnInit() {
    
    this.requestData();  /*请求数据*/
  }

  requestData(){

    var _that=this;
   
    var url='http://127.0.0.1:3000/movie?callback=JSONP_CALLBACK';
    this.jsonp.get(url).subscribe(function(result){
      

      var data=JSON.parse(result['_body']);

      _that.list=data.subjects;

      console.log(_that.list);

    },function(error){
       console.log(error);
    })

  }

}
```
2.movie.component.html
```
<header class="header">

  电影列表
</header>
<div class="content">
    <ul class="list">
      <li class="item" *ngFor="let item of list">


          <a routerLink="/movieinfo/{{item.id}}">

            <img src="{{item.images.small}}" alt="{{item.title}}">

            <div class="info">

              <h2>{{item.title}}</h2>
              <p>上映时间:{{item.year}}</p>          

              <p>主演: <span *ngFor="let cast of item.casts">{{cast.name}} </span></p>
            </div>
            </a>
      </li>
    </ul>
</div>
```
#### movieinfo
1.movieinfo.component.ts

```
import { Component, OnInit } from '@angular/core';

import { ActivatedRoute } from '@angular/router';

import { Http, Jsonp } from '@angular/http';

@Component({
  selector: 'app-movieinfo',
  templateUrl: './movieinfo.component.html',
  styleUrls: ['./movieinfo.component.css']
})
export class MovieinfoComponent implements OnInit {

  public item:any[];

  constructor(private route:ActivatedRoute,private http:Http,private jsonp:Jsonp) { }

  ngOnInit() {
    //  console.log(.value.id);

    //获取动态路由的值
    this.route.params.subscribe(function(data){
        console.log(data.id);

        this.requestData(data.id);

    }.bind(this))
  }


  requestData(id){

      // "http://127.0.0.1:3000/movieinfo?id=324324325&callback=JSONP_CALLBACK"
      
      var _that=this;
   
      var url='http://127.0.0.1:3000/movieinfo?id='+id+'&callback=JSONP_CALLBACK';
      this.jsonp.get(url).subscribe(function(result){
        
         

          

          var data=JSON.parse(result['_body']);

          _that.item=data;

    

      },function(error){
          console.log(error);
      })

  }

}
```

2.movieinfo.component.html

```
<header class="header">

 <a routerLink="/movie">
  <span class="back"></span>
 </a>
  详情
</header>
<div *ngIf="item" class="content">

    <div class="topinfo">

    <div class="item" >

          <img src="{{item.images.medium}}" alt="{{item.title}}">

              <div class="info">

                <h2>{{item.title}}</h2>              
                <p>{{item.countries[0]}}</p>         
                <p>{{item.year}}上映</p>        

                <p>主演: <span *ngFor="let cast of item.casts">{{cast.name}} </span></p>
              </div>
    </div>

  </div>

  <div class="movieinfo">

    {{item.summary}}
  </div>

    <div class="casts">

      <h3>演员列表</h3>

        <ul class="clearfix">
          <li  *ngFor="let cast of item.casts">
              <img src="{{cast.avatars.small}}" />
              <p>{{cast.name}}</p>
          </li>
        </ul>
    </div>





</div>
```
#### 页面详情
![](http://otue68nu2.bkt.clouddn.com/17-7-31/90346356.jpg)
![](http://otue68nu2.bkt.clouddn.com/17-7-31/27846692.jpg)
![](http://otue68nu2.bkt.clouddn.com/17-7-31/78847365.jpg)

```
 this.route.params.subscribe(function(data){
        console.log(data.id);

        this.requestData(data.id);

    }.bind(this))
```
点击通过this.router.params.subscribe实现获取urlID来请求api接口
