---
title: Vue基础代码整理
date: 2016-05-12 22:24:08
tags: Vue
categories: Vue
---
## 一、数据绑定
- 1.解析html标签使用三个 
    
        {{{html标签}}},{{msg}}

- 2.支持三元表达式
      
        双向绑定{{name==='hello world'?'1':'2'}}  
- 3.通过v-model实现双向数据绑定

        <input type="text" v-model="name">

## 二、vue暴露的方法
- 1.
             var vm = new Vue({
            el:'#app',
            data:message
            });
            vm.$el//拿到的就是对应的#app
            vm.$data//拿到的就是对于的data数据
            
## 三、vue的生命周期
- 1.
    ```var vm = new Vue({
        data:{
            name:'hello'
        },
         beforeCreate(){
                    console.log('实例刚刚被创建1');
                },
                created(){
                    console.log('实例已经创建完成2');
                },
                beforeMount(){
                    console.log('模板编译之前3');
                },
                mounted(){     /*请求数据放在这个里面  必须记住  mounted*/
                    console.log('模板编译完成4');
                },
                beforeUpdate(){
                    console.log('数据更新之前');
                },
                updated(){
                    console.log('数据更新完毕');
                },
                beforeDestroy(){
                    console.log('实例销毁之前');
                },
                destroyed(){
                    console.log('实例销毁完成');
                }
    });
    //手动挂载指定元素
    vm.$mount('#app');
    //销毁实例
    //vm.$destroy();```

## 四、vue的计算属性
- 1.
    ```  var vm = new Vue({
        data:{
            name:'hello'
        },
        computed:{ //有些数据是要根据我们的提供的数据计算出来的
            //默认的是获取属性 在页面中获取属性b会调用对应的方法
            b:function () {
                //当前this是vue的实例
                return this.name + ' world'
            }
            //有缓存
        }
    });
    vm.$mount('#app');```

## 五、vue的set和get
- 1.
    ```   var vm = new Vue({
        data:{
            name:'hello',
            age:'age'
        },
        computed:{
            b:{ //当给b设置属性的时候b立即会调用set方法
                set: function (val) {
                    //在这里对设置的值进行后续的处理
                    //val是设置的具体内容
                    //可以改变原数据的属性
                    //当b更改的时候才触发事件
                    this.name = val;
                    this.age = 500
                },
                get:function () {
                    return this.name + ' world'
                }
            }
        }
    });
    vm.$mount('#app');
    //调用设置值
    setTimeout(function () {
        vm.b = 'zfpx';
    },2000)```

## 六、vue的v-if和v-show(v-if和v-else以及v-show和v-hide的区别解析用法)
- 1.v-if
    v-if接收bool类型。true的话则生成html，false则不生成。或者直接将元素remove掉。

- 2.v-else
    v-else紧跟在v-if或者v-show后边，否则将不被识别

- 3.v-show
    通过此指令控制元素的显示隐藏，即控制元素的display。

- 4.v-hide
    与v-show相反.

- 5.
    ```
    1.v-if
    <div v-if="true">你好</div>

    2.v-else
    <div v-if="flase">错的</div>
    <div v-else>不是错的</div>

    3.v-show
    <div v-show="true" style="display:none">我显示</div>
    <div v-show="false" style="display:none">我隐藏</div>
    ```

## 七、vue的数据遍历
- 1.v-for="book in books"(数组用$index,对象用$key)
    ```<div id="app">
    <div v-for="book in books">
        {{$index}}{{book.name}} {{book.price}}
    </div>
</div>
<script src="vue.js"></script>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            books:[
                {name:'javascript',price:50},
                {name:'node',price:50},
                {name:'ajax',price:50},
                {name:'vue',price:50},
            ]
        }
    })
    <!--输出0javascript 50
    1node 50
    2ajax 50
    3vue 50-->
</script>```

## 八、vue的双重循环
- 1.v-for="(index,book) in books"
    ```<div id="app">
        <!--可以用括号的形式 第一个参数是当前索引 第二个是数组的每一项   index = $index-->
        <div v-for="(index,book) in books">
            {{book.name}} {{index}}
            <!--book代表数组的每一项-->
            <div v-for="b in book.name">
                父亲{{index}}{{$index}}
            </div>
        </div>
    </div>
    <script src="vue.js"></script>
    <script>
        var vm = new Vue({
            el:'#app',
            data:{
                books:[
                    {name:['a','e','f'],price:50},
                    {name:['a'],price:50},
                    {name:['a','e'],price:50},
                    {name:['a','e','a','e'],price:50},
                ]
            }
        })
    </script>```

## 九、vue的v-bind
- 1.v-bind 绑定动态数据 简写:
    ``` var vm = new Vue({
            el:'#app',
            data:{
                src:'http://vuejs.org/images/logo.png',
                src1:'http://www.baidu.com'
            },
            //当前要执行的方法写在methods中 用@符号或者v-on进行绑定
            //当我们不进行参数的传递 不用写()如果要传递参数
            methods:{
                //如果有参数 需要手动传递event
                dosome: function (e,ev) {
                    console.log(ev);
                    alert(1);
                }
            }
        });```

## 十、vue的class
- 1.
    ```<div id="app">
    <div class="{{style}}">123</div>
    //直接动态引入数据
    <div v-bind:class="style">123</div>
    //不是true就是false
    <div v-bind:class="{'red':true,'yellow':true}">123</div>
    //绑定数组
    <div :class="['red','yellow']">536</div>
    //三元表达式写法
    <div :class="['red',flag?'orange':'yellow']">536</div>
    //最简单的用法 数组和对象并用
    <!--<div class='color' ng-class="{red:flag}"></div>-->
    <div :class="['red',{yellow:flag},flag?'orange':'yellow']">789</div>
    <!--class="{{className}}" 和v:bind:class='obj' 不要混用-->
    <!--class 可以和v-bind:class共存-->
</div>
<body>
<script src="vue.js"></script>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            style:'red',
            flag:true
        }
    });
</script>```

## 十一、vue的表单
- 1.
        ```<div id="app" >
        //取value值 对应的checkbox 必须是数组 <br>
        <input checked type="checkbox" value="点击" v-model="name" name="all">
        <input type="checkbox" value="点击1" v-model="name" name="all">
        <input type="checkbox" value="点击2" v-model="name" name="all">
        {{name}}

        <input type="radio" v-model="radio" value="man" name="group">男
        <input type="radio" v-model="radio" value="woman" name="group">女
        {{radio}}
        //取selects的值就是我们对应的当前选中的值
        <select v-model="selects">
            <option v-for="l in lists" v-bind:value="l.name">{{l.name}}</option>
            <!--<option value="1">hello</option>
            <option value="2">world</option>
            <option value="3">hello world</option>-->
        </select>

    <!-- <select id="select">
            <option value="1">4</option>
            <option value="2">3</option>
            <option value="3">2</option>
        </select>-->


        <input type="checkbox" v-model="name">{{name}}
    </div>
    <script src="vue.js"></script>
    <script>
    /* select.onchange = function () {
            alert(this.value)
        }*/
        var vm = new Vue({
            el:"#app",
            data:{
            name:[],
            selects:'jw2',
            lists:[
                    {name:'jw1'},
                    {name:'jw2'},
                    {name:'jw3'}
            ]
            }
        })
    </script>```
## 十二、vue的语法糖父子组件传值验证
- 1.
    ```<div id="app">
    <!--<table>
        <tr is="parent"></tr>
    </table>-->
    //给parent 模板传递了一个msg变量
    <!--message = who ar you-->
    <!--:message="'1'"-->
    <!--//这里是原本界面的data -->
    全局下的<input type="text" v-model="data">
    <!--//默认是单向数据流-->
    <parent :message.sync="data"></parent>
    <template id="parent"><br>
        parent<input type="text" v-model="message">
        <div>parent {{message}} {{message2}}</div>
        <div>parent</div>
        <ch></ch>
    </template>
    <template id="child">
        <div>child</div>
        <div>child</div>
    </template>
</div>
<script src="vue.js"></script>
<script>
    //我们想传递数据  先从属性 传递到props  在从props取出来绑定到组件里
    //:message="1" 是绑定的数字1
    //message="1" 绑定的字符串1 只能传递字符串1

    //组件的嵌套
    //先创建自组件，子组件创建后插到父组件里

    //vuex
    Vue.component('parent',{
        template:'#parent',
        props:{ //对属性进行验证
            'message':{
                type:[String], //只能传递字符串类型的
                default: function () {
                    return 100 //默认值可以不符合type类型
                },
                twoWay:true, //要求必须是双向的
                validator: function (v) {//不写的话没有校验器
                    //v就代表传进来的值 校验v的值是否是你的预期
                    return true
                    //v==='who are you6000'
                },
                coerce: function (v) {
                    console.log(v);
                    //传进来的值 强制进行改变 在验证之前
                    return v;//之后开始检测type是否符合
                }
            },
            'message2':{
                default: function () {
                    return 200
                },
                coerce: function (v) {
                    console.log(v);
                    //传进来的值 强制进行改变 在验证之前
                    return 2000;//之后开始检测type是否符合
                }
            }
        }, //属性
        components:{
            ch:{
                template:'#child'
            }
        }
    });
    //组件和数据交互




    /*var child = Vue.extend({
        template:'<div>child</div>'
    });
    //只能在父组件里使用
    var parent = Vue.extend({
        template:'<div>parent</div><ch></ch>',
        components:{
            ch:child
        }
    });
    //只注册了父亲组件
    Vue.component('parent',parent);*/


    /*Vue.component('com',{
      template:'<div>hello</div>'
    });*/
    var vm = new Vue({
        el:'#app',
        data:{
            data:'who are you'
        }
    });
</script>```

## 十三、vue的组件
- 1.
    ```<div id="app">
    <hello></hello>
    <hello></hello>
    <hello></hello>
</div>
<template id="tmpl">
    <ul>
        <li>home {{name}}</li>
        <li>profile</li>
        <li>concat</li>
    </ul>
</template>
<script src="vue.js"></script>
<script>
    //把html取出来当作模板，用数据渲染好，将我们的组件扔到页面里
    //用extend方法创建出组建,在实例创建前
    var hello = Vue.extend({
        //template就是组件的模板
        template:'#tmpl'
    });
    var world = Vue.extend({
        //template就是组件的模板
        template:'<div>world</div>'
    });
    //注册组件 第一个参数是组件的名字，第二个参数是创建的组件
    Vue.component('hello',hello);
    Vue.component('world',world);
    var vm = new Vue({
        el:'#app',
    });
</script>```

## 十四、vue的动态组件
- 1.
    ```<div id="app">
    <input type="radio" value="home" v-model="current" name="rad">home
    <input type="radio" value="page" v-model="current" name="rad">page
    <component :is="current"></component>
</div>
<script src="vue.js"></script>
<script>
    var vm = new Vue({
        el:'#app',
        data:{
            current:''
        },
        components:{
            home:{
                template:'<div>Hello</div>'
            },
            page:{
                template:'<div>world</div>'
            }
        }
    })
</script>```