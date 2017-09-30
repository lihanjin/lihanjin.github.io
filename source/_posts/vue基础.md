---
title: Vue基础整理
date: 2016-05-11 22:24:08
tags: Vue
categories: Vue
---
## Vue基础整理



***
### 1、先看一个经典的例子
    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8" />
            <title></title>
            <script src="https://unpkg.com/vue/dist/vue.js"></script>
        </head>
        <body>
            <div id="app" v-on:click="fn1" >{{message}}</div>
            <script>
                var app = new Vue({
                    el:"#app",
                    data:{
                        message: 'Hello Vue.js!'
                    },
                    methods:{
                        fn1: function(){
                            this.message = 'hello world'
                        }
                    }
                })
            </script>
        </body>
    </html>
运行上面的代码 页面上就会输出 "Hello Vue.js! " 几个字，点击文字会变成"hello world"
### 1-1我们来理解一下vue的执行机制

```
<div id="app" v-on:click="fn1" >{{message}}</div>
<script>
    var app = new Vue({
        el:"#app", //可以用id(#)、类(.)、标签(div)
        data:{
            message: 'Hello Vue.js!'
        },
        methods:{
            fn1: function(){
              this.message = 'hello world'
            }
        }
})
</script>
```


上面的代中，用new关键字实例化了一个Vue对象并传递了一个参数（对象）这个对象主要包括3个重要的方法`(el,data,methods)`
el选择要渲染的元素，`el:"#app" el:".app" el:"div"` 这些都是支持的
data 是一个对象，里面存放数据（或者说变量）,他可以直接显示在页面上，可以在`methods`中被调用、修改。
`methods` 这里面是一些方法的集合，把所有的方法都定义在`mehtods`中.，再通过`v-on:click="fn1"`的方式调用`mehtods`中的`fn1`方法。

总结一下

1/首先得有一个标签（展示数据） 如: `<div id="app">{{message}}</div>`
2/其次得实例化一个vue对象 new  Vue()
3/在传入的对象中

3-1选择元素
`el:"#app"`

3-2定义数据

```
data:{
       message: 'Hello Vue.js!'
}
```

3-3定义方法

```
methods:{
    fn1: function(){
        this.message = 'hello world'
    }
}
```




### 1-2、数据绑定

        <!--
    src 是 HTML的属性，{{}} 是 ng 的表达式, 表达式可用于很多地方，包含属性，所以直
    接 src="{{vm.url}}" 其实就是使用Vue的表达式给属性赋值，这种做法的缺点是当第一
    次加载模板的时候浏览器会去请求 “{{vm.url}}” 的地址，当Vue编译模板后把
     {{vm.url}} 替换成对应的URL后会再次请求真实的地址，所以为了避免第一次无效的
     请求，Vue 自带了v-bind:src 指令，其实和v-bind:href的原理类似。

        -->
1.`v-bind:id="id"`绑定id
2.`v-bind:title:"msg`"绑定title
3.`v-bind:src:"url"`绑定src
4.`v-text:"msg"`绑定文本
简写：
`v-on:click------@click`
`v-bind:src------:src`

### 1-3、事件绑定

在vue中给一个元素绑定事件可以用 v-on:+事件名称（click、mouseover、mouseout、keyup、keydown 等），v-on:这种写法有些繁琐，v-on:可以用 @符代替

```
<div v-on:click="fn1" ></div>
<div @click="fn1" ></div>
//以上两种绑定事件的方式是等价的
```
### 1-4、vue中event事件对象
现在比如有这样一个需求，点击按钮要获取鼠标相对于浏览器的x和y坐标，原生js中只需要给点击方法传一个event对象，通过event对象来获取相应的值，vue中也提供了一个类似的方法 $event，vue多一个$符号。
`ev.cancelBubble = true`阻止事件冒泡
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <script src="https://unpkg.com/vue/dist/vue.js"></script>
    </head>
    <body>
        <button id="app" @click="fn1($event)">{{message}}</button>  
        //这里的fn1方法传递了一个$event参数,和原生event类似
        <script>
            var app = new Vue({
                el:"#app",
                data:{
                     message: '点我'
                },
                methods:{
                    fn1: function(ev){
                      console.log(ev.clientX +":"+ev.clientY ) //在控制台中查看x 和y坐标的值
                    }
                }
            })
        </script>
    </body>
</html>
```

### 1-5、vue中的keyCode
在日常开发中，最常见的一个小需求（回车提交），当用户按了回车键就提交用户填写的数据，在原生js中还是要依靠event事件对象，通过event来获取keyCode，记住keyCode不是一件容易的事所以 Vue 为最常用的按键提供了别名，目前vue提供了下面这样键盘别名：
```

.enter 回车键
.tab tab切换
.delete (捕获 “删除” 和 “退格” 键)
.esc esc键
.space 空格键
.up 键盘上键
.down 键盘下键
.left 键盘左键
.right 键盘右键
```

### 1-6、vue中的表达式

```
  {{number*20}} 
  {{ok?'111':'222'}}
  {{message.split('').reverse().join('')}}
```


### 1-7、vue中的计算属性

```
computed:{
	a:function(){
	retrurn this.msg+"这是计算后的属性"
	}
}//写在methods的同级
```
`{{a}}`使用计算属性和使用data绑定的值一样，用到的data里面绑定的值变化，他就会变化


### 1-8、vue中的$watch（watch） 监听数据变化
- 1.第一种写法

```
 vm.$watch('name',function(newValue,oldValue){
			//name是state里面的数据
			//newValue是state变化后的新值
			//oldValue是state变化之前的值
})
```
- 2.第二种写法

```
 watch:{
        msg:function(newVal,oldVal){
            console.log(newVal+'--'+oldVal);
        }
    }
```
### 1-9、vue中的v-bind:class
- 可以通过布尔值来控制class
```
<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }">
```


- 第一个元素加red的class

```
list:['1','2','3']

<li v-for="(item,key) in list" :class={"red":key==0}>{{item}</li>
```
- 可以把一个数组传给v-bind:class，以应用一个class列表

```
<div id="app">
    <label v-bind:class="[activeClass, errorClass]">测试</label>
</div>
    var app = new Vue({
        el:'#app',
        data:{
            activeClass:'active',
            errorClass:'text-danger'
        }
    })
```
- 也可以使用三元表达式

```
 <label v-bind:class="[isActive ? activeClass : '', errorClass]">测试</label>
```
- 始终添加一个和判断添加一个

```
<label v-bind:class="[{active:isActive}, errorClass]">测试</label>
```
- 可以绑定v-style

```
	<div id="app">
    <label v-bind:style="classObj">测试</label>
</div>
    var app = new Vue({
        el:'#app',
        data:{
            classObj: {
                color: 'red',
                fontSize: '30px'
            }
        }
    })
```

### 1-10、vue中的数据请求插件
vue-resource        axios

### 1-11、vue中的双向绑定数据

```
input  type="text"    v-model

	checkbox
		
	radio       v-model


select   值
```

