---
title: Vue基础总结
date: 2016-05-11 22:24:08
tags: Vue
categories: Vue
---
## Vue基础整理

# 0.单文件组件的使用：



## 一、全局安装 vue-cli
	$ npm install --global vue-cli



### 二、创建一个基于 webpack 模板的新项目


	$ vue init webpack-simple my-project



### 三、安装依赖，走你 


	$ cd my-project


	$ npm install


	$ npm run dev



************************************************************************************************************

1.数据绑定     *

    {{name}}    
    
    
    <sapn v-text="name"></span>


    绑定html：

    
    <sapn v-html="name"></span>


2.定义数据  data   *              组件里面注意



```
 var vm=new Vue({
    data:{
        msg:'数据'
    }
 })
```

组件：

```
data(){
   return {
	 msg:'数据'
  }

}
```





3.定义方法  methods  *


```
 var vm=new Vue({
    data:{
        msg:'数据'
    },methods:{
            getData:function(){

            }
    }
 })
```


4.绑定 动态属性  *

    v-bind:src='src'

    :src='src'

    <img :src="url" alt="">



  <div id="box"></div>

  
   


5.执行事件的两种方式  *

    v-on:click="run()"

    @click="run()"






6.计算属性computed  





```
{{{a}}


 var vm=new Vue({
    data:{
        msg:'数据',
	list:['apple','bbbb','cccsss']
    },methods:{
            getData:function(){

            }
    },
    computed:{
        a:function(){
            return this.msg+'这是计算后的属性'

        }
    }
 })
```




使用计算属性和使用data绑定的值一样

用到的data里面绑定的值变化  他就会变化


{{a}}






7. watch  $watch  监听数据变化


```
    var vm=new Vue({
        el:"#box",
        data:{
            msg:'this  is a msg',
            name:''
        },
        methods:{

            getData:function(){

              console.log(this.name);
            }
        }
    })

    vm.$watch('name',function(newValue,oldValue){

            console.log(newValue+'---'+oldValue);

    })


 var vm=new Vue({
    el:'#box',
    data:{
       msg:'我是一个数据'
    },methods:{

        requestData:function(){
            console.log(this.msg);
            /*请求数据*/

        }
    },
    watch:{
        msg:function(newVal,oldVal){
            console.log(newVal+'--'+oldVal);
        }
    }
});
```

8.  v-bind:class   *

```
 <div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }">
```




	第一个元素加red的class

	list:['1','2','3']



	<li v-for="(item,key) in list" :class={"red":key==0}>{{item}</li>






9.v-if  v-show          v-if v-else                      *


	v-if  dom操作

	v-show  显示隐藏




10.数据循环            *

```
 <li v-for="(val,index) in list">
    {{val}} ---{{index}}
</li>
```



```
<div v-for="item of items"></div>
```



11.数据请求 插件必须记住         *


`vue-resource`             `axios`




12.事件对象   可以获取自定义属性的值         获取元素         *


 

```
 <div class="button" data-id="123"  data-aid="456"  @click="show()"> 这是一个div</div>
```




13.键盘事件


	可以用事件对象获取按的那个键盘

	
14.双向数据绑定  *
	

```
input  type="text"    v-model

	checkbox
		
	radio       v-model


select   值
```

		
 


15.生命周期函数   mounted    *

```
  new Vue({
    el:'#box',
    data:{
        msg:'welcome vue2.0'
    },
    methods:{
        update:function(){
            this.msg='大家好111';
        },add:function(){

        }
    },

mounted:function(){     /*实例创建完成并且模板编译完成   加载的时候默认触发的方法*/

        console.log('111111111111');


        this.update();
    }


});



componentDidMount  react
```


 16.组件是怎么理解的？

    定义注册组件的几种方式：      必须记住



********************************************************************************************************
    1.定义组件

   

```
 var Header={
            template:'<h2>这是一个头部组件</h2>',           /*模板里面必须有根元素*/
            data:function(){
                    return:{
                          msg:'xxx'
                    }
            },
            methods:{
            }
    }

    2.注册组件

     Vue.component('v-header',Header);
```

********************************************************************************************************

```
1.定义组件 注册组件放在一起


 Vue.component('v-header',{
         template:'<h2>这是一个头部组件</h2>'
  });
```

********************************************************************************************************


    1.定义组件               ************************最常用的方式

   

```
 var Header={
            template:'<h2>这是一个头部组件</h2>'
    }

    2.注册组件

      var vm=new Vue({
        el:'#box',
        data:{
            name:'zhangsan'
        },
        components:{   /*2.注册组件*/
            'v-header':Header
        }

    });
```

********************************************************************************************************


   1.定义组件 注册组件放在一起

```
  var vm=new Vue({
    el:'#box',
    data:{
        name:'zhangsan'

    },
    components:{   /*2.注册组件*/
        'v-header':{
               template:'<h2>这是一个头部组件</h2>'
         }

    }

props:['name','a']

});
```

********************************************************************************************************

 17.父子组件的通信


        1.父组件给子组件传值：
        {
            1.子组件定义props接收父亲组件传过来的值	props:['name','a']

            2.父组件调用子组件的时候给定义的属性传值<v-header :name="name" :a="age"></v-header>

        }


         2.子组件给父组件传值：
                {

                    1.子组件  this.$emit('parent',this.name)   子组件向父组件广播数据

                    2.父组件 调用子组件的时候 监听广播准备触发方法  <v-nav @parent="getChildData"></v-nav>   v-nav是父亲组件的子组件

                    3.父组件

                        getChildData:function(data){   /*子组件广播数据触发的方法*/
                               //alert(data);
                               this.msg=data;
                        }

         }

        3.兄弟组件的传值

               1.实例化vue     var Event=new  Vue();

               2.传值的话广播  A给B传值  A里面广播。  Event.$emit('to-b',数据)

               3.A给B传值  A里面广播  B接收数据     注意：接收事件要注册到生命周期函数 mounted

                Event.$on('to-b',function(data){})


        4.父组件主动获取子组件的数据和方法  *


                1.父组件调用子组件的时候定义有个 ref       <v-nav ref="myNav"></v-nav>

                2.this.$refs.myNav.子组件数据                 this.$refs.myNav.子组件方法


        5..子组件主动获取父组件的数据和方法   *


            this.$parent.父组件的数据            this.$parent.父组件的方法





 18.动画     div  或者路由显示 隐藏的时候会触发
  


      <button @click="flag=!flag">点击显示隐藏</button>

      <transition name="fade">
                <div class="box" v-if="flag">
                   这是一个div
                </div>
      </transition>


    .fade-enter-active{

       帧动画
    }

    .fade-leave-active{

       帧动画
    }




    key：唯一


    transition
    transition-group



    <transition-group enter-active-class="animated zoomInLeft" leave-active-class="animated zoomOutRight">
               <p v-show="show" :key="1">这是一个p标签</p>

               <p v-show="show" :key="2">这是一个p标签</p>
    </transition-group>





 19.路由

	//https://router.vuejs.org/zh-cn/

	1.安装vue-router      npm install vue-router  --save

	2.引入vue-router
		import VueRouter from 'vue-router'

		Vue.use(VueRouter)

	3.创建组件，引入组件


	4.配置路由
		const routes = [
		  { path: '/home', component: Home },
		  { path: '/news', component: News }
		]



	5.实例化VueRouter

		const router = new VueRouter({
		  routes // （缩写）相当于 routes: routes
		})

	6.挂载到vue实例上面

		new Vue({
			  el: '#app',
			  router,
			  render: h => h(App)
		})

	7. App.vue一定注意配置 组件显示的视图

		<router-view></router-view>



    <router-link to="/home">首页</router-link>
    <router-link to="/news">新闻</router-link>




20. 路由嵌套

https://router.vuejs.org/zh-cn/essentials/nested-routes.html


```
  {
    path:'/home',
    component:Home,
    children:[  /*定义Home路由的子路由*/
        {'path':'welcome',component:Welcome},
        {'path':'homeinfo',component:HomeInfo}
    ]
}
```

注意：父组件的视图里面要加入`<router-view></router-view>`














Mint-UI移动端的组件库



http://mint-ui.github.io/#!/zh-cn

Element UI


http://element.eleme.io/#/zh-CN






Mint-UI 安装使用


1.安装mint-ui


	npm install mint-ui -S   /  cnpm install mint-ui -S


	-S表示--save

2.引入mint-ui


	import MintUI from 'mint-ui'

	import 'mint-ui/lib/style.css'


	Vue.use(MintUI)


3.注意配置css-loader style-loader

	{
		test: /\.css$/,
		use: [
		  { loader: "style-loader" },
		  { loader: "css-loader" }
		]
	      }

4.看文档使用。




https://github.com/ElemeFE/mint-ui





在 mintUi的组建上面出发方法  

 @click.native=""






vue路由js跳转 

	 http://blog.csdn.net/sinat_17775997/article/details/68941091

	this.$router.push({path: '/login?url=' + this.$route.path});



	首页：

	this.$router.push({path: '/home'});




