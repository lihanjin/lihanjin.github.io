---
title: Vue官网基础整理
date: 2017-05-12 22:24:08
tags: Vue
categories: Vue
---
##VUE2.0总结


----------
### 一.vue实例


----------
**创建一个vue的实例**
每个vue应用都是通过vue函数创建的一个新的vue实例开始的：
```
var vm = new Vue({
	//选项
})
```


----------


**数据与方法**
当一个Vue实例被创建时，它项Vue的响应式系统中加入了其data对象中能找到的缩影的属性。当这些数据的值发生改变是，视图将会产生'响应'，即匹配更新为新的值。

```
// 我们的数据对象
var data = { a: 1 }
// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})
// 他们引用相同的对象！
vm.a === data.a // => true
// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2
// ... and vice-versa
data.a = 3
vm.a // => 3
```
只有在data中存在的属性是响应式的，所以可以在一开始给data附一个初始值。
除了data属性，vue实例暴露了一些其他的方法和属性，他们加上了$前缀，以便与用户定义的属性区分开来，例如:

```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})
vm.$data === data // => true
vm.$el === document.getElementById('example') // => true
// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```


----------
**实例生命周期**
![enter image description here](https://cn.vuejs.org/images/lifecycle.png)
http://blog.csdn.net/qq_21439971/article/details/76502598详细链接
1. `create` 和 `mounted`

`beforecreated：el` 和 `data` 并未初始化
`created`:完成了 `data` 数据的初始化，`el`没有
`beforeMount`：完成了 `el` 和 `data` 初始化
`mounted` ：完成挂载
另外在标红处，我们能发现el还是 `{{message}}`，这里就是应用的 Virtual DOM（虚拟Dom）技术，先把坑占住了。到后面mounted挂载的时候再把值渲染进去。

2. update

我们单击页面中的“更新数据”按钮，将数据更新。下面就能看到data里的值被修改后，将会触发update的操作。
ps:注意beforeUpdate是指view层的数据变化前，不是data中的数据改变前触发。因为Vue是数据驱动的。注意观察弹窗就容易发现。


3. destroy

销毁完成后，我们再重新改变message的值，vue不再对此动作进行响应了。但是原先生成的dom元素还存在，可以这么理解，执行了destroy操作，后续就不再受vue控制了。因为这个Vue实例已经不存在了。
我们单击页面中的“销毁”按钮，将指定的Vue实例销毁。

三、生命周期总结
`beforecreate` : 举个栗子：可以在这加个loading事件
`created` ：在这结束loading，还做一些初始化，实现函数自执行
`mounted` ： 在这发起后端请求，拿回数据，配合路由钩子做一些事情
`beforeDestory`： 你确认删除XX吗？ destoryed ：当前组件已被删除，清空相关内容

### 二.模板语法
Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML ，所以能被遵循规范的浏览器和 HTML 解析器解析。
在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，在应用状态改变时，Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上。
如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，直接写渲染 (render) 函数，使用可选的 JSX 语法。


----------
**插值**
1.**文本**
数据绑定文本用Mustache"语法(双大括号)的文本插值:
`<span>msg:{{msg}}</span>`
通过`v-once`指令可以一次性插值，当文本改变不会响应改变插值
`<span v-once>msg:{{msg}}</span>`
2.**原始HTML**
真正的html输出需要v-html
`<div v-html="rawHtml"></div>`
3.**特性**
mustache语法不能作用在html的特性上，需要使用v-bind,
`<div v-bind:id="dynamicId"></div>`
这同样适用于布尔类特性，如果求值结果是 falsy 的值，则该特性将会被删除：
`<button v-bind:disabled="isButtonDisabled">Button</button>`
4.**使用JavaScript表达式**
vue提供了完全的JavaScript表达式的支持。
**每个绑定的表达式只能是单个的表达式**

```
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
//下面语句不会生效
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}
<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```
5.**指令**
指令是带有`v-`前缀的特殊属性。指令的职责是，当表达式的值改变是，将其产生的连带影响，响应式的作用于DOM。
例如:v-if  v-for v-bind 等等
6.**参数**
一些指令能够接受一个参数，在指令的名称之后以冒号表示，例如:
`<a v-bind:href="url"></a>`
这里href是参数，告知v-bind指令将该元素的href属性与表达式的url绑定。
7.**修饰符**
修饰符是以`.`半角句号指明的特殊后缀，用于指出一个指令应该以什么方式绑定，例如:.stop .prevent等等
8.**缩写**
`v-bind` 缩写

```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

`v-on` 缩写

```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```
### 三.计算属性和观察者


----------
**计算属性**
模板的表达式只适用于简单的表达式，如果有更复杂的计算需要使用compute计算属性
1.**基础例子**

```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```
2.**计算属性的缓存vs方法**
你可能已经注意到我们可以通过在表达式中调用方法来达到同样的效果：

```
<p>Reversed message: "{{ reversedMessage() }}"</p>
// in component
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
使用计算属性会有缓存，因为计算属性是依赖缓存来进行缓存的。如果不需要缓存，用方法代替。
3.**计算属性VS被观察的属性**
Vue.js 提供了一个方法 watch，它用于观察Vue实例上的数据变动。对应一个对象，键是观察表达式，值是对应回调。值也可以是方法名，或者是对象，包含选项。具体的用法可以直接看下面的示例，简单直接。

```
[html] view plain copy
<span style="color:#006600;"><div id="app">  
    <input type="text" v-model:value="childrens.name" />  
    <input type="text" v-model:value="lastName" />  
</div>  
  
<script type="text/javascript">     
    var vm = new Vue( {  
        el: '#app',  
        data: {  
            childrens: {  
                name: '小强',  
                age: 20,  
                sex: '男'  
            },  
            tdArray:["1","2"],  
            lastName:"张三"  
        },  
        watch:{  
            childrens:{  
                handler:function(val,oldval){  
                    console.log(val.name)  
                },  
                deep:true//对象内部的属性监听，也叫深度监听  
            },  
            'childrens.name':function(val,oldval){  
                console.log(val+"aaa")  
            },//键路径必须加上引号  
            lastName:function(val,oldval){  
                console.log(this.lastName)  
            }  
        },//以V-model绑定数据时使用的数据变化监测  
    } );  
    vm.$watch("lastName",function(val,oldval){  
        console.log(val)  
    })//主动调用$watch方法来进行数据监测</span>  
</script>  
</body>  
```

注意：数组的改变不需要使用深度watch。
4**计算属性的setter**
计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：

```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

现在再运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也相应地会被更新。
### 四.Class与Style绑定


----------
**绑定HTML Class**
1.**对象语法**
可以通过v-bind:class一个对象动态切换class：
`<div v-bind:class="{ active: isActive }"></div>`
判断isActive是否为真值
也可以直接绑定一个对象

```
<div v-bind:class="classObject"></div>
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
或者可以绑定一个计算属性

```
<div v-bind:class="classObject"></div>
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
2**数组语法**
我们可以把一个数组传给v-bind:class，以应用一个class列表：

```
<div v-bind:class="[activeClass, errorClass]"></div>
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

渲染为：
`<div class="active text-danger"></div>`
如果你也想根据条件切换列表中的 class，可以用三元表达式：
`<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>`
此例始终添加 errorClass ，但是只有在 isActive 是 true 时添加 activeClass。
不过，当有多个条件 class 时这样写有些繁琐。可以在数组语法中使用对象语法：
`<div v-bind:class="[{ active: isActive }, errorClass]"></div>`


----------
**绑定内联样式**
1.**对象语法**
v-bind:style 的对象语法十分直观——看着非常像 CSS，其实它是一个 JavaScript 对象。CSS 属性名可以用驼峰式 (camelCase) 或 (配合引号的) 短横分隔命名 (kebab-case)：

```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
data: {
  activeColor: 'red',
  fontSize: 30
}
```

直接绑定到一个样式对象通常更好，让模板更清晰：

```
<div v-bind:style="styleObject"></div>
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

同样的，对象语法常常结合返回对象的计算属性使用。
2.**数组语法**

v-bind:style 的数组语法可以将多个样式对象应用到一个元素上：
`<div v-bind:style="[baseStyles, overridingStyles]"></div>`
3.**自动添加前缀**

当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。
4.**多重值**


从 2.3.0 起你可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：
`<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>`
这会渲染数组中最后一个被浏览器支持的值。在这个例子中，如果浏览器支持不带浏览器前缀的 flexbox，那么渲染结果会是 display: flex。
### 五.条件渲染


----------
v-if

```
<h1 v-if="ok">Yes</h1>
也可以用 v-else 添加一个 “else” 块：
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```
还有 v-else 和 v-else-if
可以用key来管理可以复用的元素
使元素全部重新渲染

v-show
v-show只是切换元素的display属性，v-show渲染的元素始终会被渲染并保留在DOM中。
tips：v-if和v-for一起使用过的时候 v-for比v-if有更高的优先级
### 六.列表渲染


----------
1.**用v-for把一个数组对应为一组元素**
可以用 v-for item in items    (item,index) in items   或者 v-for of

对象遍历

```
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```
**key**
当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。这个类似 Vue 1.x 的 track-by="$index" 。
这个默认的模式是高效的，但是只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。
为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。理想的 key 值是每项都有的且唯一的 id。这个特殊的属性相当于 Vue 1.x 的 track-by ，但它的工作方式类似于一个属性，所以你需要用 v-bind 来绑定动态值 (在这里使用简写)：


----------
**数组的更新监测**
1.**变异方法**
vue数组的
- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()
都会触发视图更新
2.**替换数组**
变异方法 (mutation method)，顾名思义，会改变被这些方法调用的原始数组。相比之下，也有非变异 (non-mutating method) 方法，例如：filter(), concat() 和 slice() 。这些不会改变原始数组，但总是返回一个新数组。当使用非变异方法时，可以用新数组替换旧数组
3.**注意事项**
1.利用索引直接设置一个项时，不会触犯状态更新`vm.items[indexOfItem] = newValue`
2.改变数组长度时候也不会粗发状态更新。
第一个问题的解决办法2中

```
// Vue.set
Vue.set(example1.items, indexOfItem, newValue)
// Array.prototype.splice
example1.items.splice(indexOfItem, 1, newValue)
```
第二类问题可以使用splice

```
example1.items.splice(newLength)
```


----------
对象更改检测注意事项
还是由于 JavaScript 的限制，Vue 不能检测对象属性的添加或删除：
已经创建的实例需要使用vue.set(object,key,value)方法向嵌套对象添加响应式属性。还可以使用vm.$set实例方法，它只是Vue.set的别名：
如果需要个已有的对象赋予多个新属性，比如使用Object.assign()或_extend().
不要像这样:
```
Object.assign(this.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

你应该这样做：

```
this.userProfile = Object.assign({}, this.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```


----------
**显示过滤/排序结果**
有时，我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。
例如：

```
<li v-for="n in evenNumbers">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法：

```
<li v-for="n in even(numbers)">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
```

### 七.事件处理


----------
可以用v-on监听事件和触发一些javascript代码。
如果需要访问原生的DOM事件。可以使用特殊变量$event把它传入方法：


----------
**事件修饰符**.stop
- .prevent
- .capture
- .self
- .once

```
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身 (比如不是子元素) 触发时触发回调 -->
<div v-on:click.self="doThat">...</div>
```


----------
**键值修饰符**
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right

### 八.表单输入绑定


----------
**基础用法**
可以使用v-model实现双向数据绑定


----------
**修饰符**
.lazy

在默认情况下，v-model 在 input 事件中同步输入框的值与数据 (除了 上述 IME 部分)，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步：

```
<!-- 在 "change" 而不是 "input" 事件中更新 -->
<input v-model.lazy="msg" >
.number
```

如果想自动将用户的输入值转为 Number 类型 (如果原值的转换结果为 NaN 则返回原值)，可以添加一个修饰符 number 给 v-model 来处理输入值：

```
<input v-model.number="age" type="number">
```

这通常很有用，因为在 type="number" 时 HTML 中输入的值也总是会返回字符串类型。
.trim

如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入：

```
<input v-model.trim="msg">
v-model 与组件
```

### 九.组件


----------
**使用组件**
1.**注册**
要注册一个全局组件可以使用Vue.component(tagName,options)
```
Vue.component('my-conmponent',{
	//选项
})
```
组件在注册之后，便可以在父实例的模块中以自定义元素 `<my-component></my-component>` 的形式使用。要确保在初始化根实例之前注册了组件：

```
<div id="example">
  <my-component></my-component>
</div>
// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})
// 创建根实例
new Vue({
  el: '#example'
})
```



2.**局部注册组件**

```
var Child = {
  template: '<div>A custom component!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})
```



3.**data必须是函数**


4.**组合组件**
组件意味着协同工作，通常父子组件会是这样的关系：组件 A 在它的模板中使用了组件 B。它们之间必然需要相互通信：父组件要给子组件传递数据，子组件需要将它内部发生的事情告知给父组件。然而，在一个良好定义的接口中尽可能将父子组件解耦是很重要的。这保证了每个组件可以在相对隔离的环境中书写和理解，也大幅提高了组件的可维护性和可重用性。
在 Vue 中，父子组件的关系可以总结为 props down, events up。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。看看它们是怎么工作的。
![enter image description here](https://cn.vuejs.org/images/props-events.png)


----------
**Props**
1.**使用Props传递数据**
要让子组件使用父组件的数据，我们需要通过props选项。
子组件要显式地用 props 选项声明它期待获得的数据：

```
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 可以用在模板内
  // 同样也可以在 vm 实例中像“this.message”这样使用
  template: '<span>{{ message }}</span>'
})
```

然后我们可以这样向它传入一个普通字符串：

```
<child message="hello!"></child>
```


----------
2.**动态props**
在模板中，要动态地绑定父组件的数据到子模板的 props，与绑定到任何普通的 HTML 特性相类似，就是用 v-bind。每当父组件的数据变化时，该变化也会传导给子组件：

```
<div>
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>
```

使用 v-bind 的缩写语法通常更简单：

```
<child :my-message="parentMsg"></child>
```

3.`单向数据流`

prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解。
另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop。如果你这么做了，Vue 会在控制台给出警告。
为什么我们会有修改 prop 中数据的冲动呢？通常是这两种原因：
prop 作为初始值传入后，子组件想把它当作局部数据来用；
prop 作为初始值传入，由子组件处理成其它数据输出。
对这两种原因，正确的应对方式是：
定义一个局部变量，并用 prop 的值初始化它：

```
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}
```

定义一个计算属性，处理 prop 的值并返回。

```
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

4.**Prop 验证**

我们可以为组件的 props 指定验证规格。如果传入的数据不符合规格，Vue 会发出警告。当组件给其他人使用时，这很有用。
要指定验证规格，需要用对象的形式，而不能用字符串数组：

```
Vue.component('example', {
  props: {
    // 基础类型检测 (`null` 意思是任何类型都可以)
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组/对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

type 可以是下面原生构造器：
String
Number
Boolean
Function
Object
Array
Symbol
type 也可以是一个自定义构造器函数，使用 instanceof 检测。
当 prop 验证失败，Vue 会抛出警告 (如果使用的是开发版本)。注意 props 会在组件实例创建之前进行校验，所以在 default 或 validator 函数里，诸如 data、computed 或 methods 等实例属性还无法使用。


----------
**自定义事件**
子组件和父组件通讯
每个 Vue 实例都实现了事件接口 (Events interface)，即：
使用 `$on(eventName)` 监听事件
使用 `$emit(eventName)` 触发事件
**绑定原生事件**
有时候，你可能想在某个组件的根元素上监听一个原生事件。可以使用 .native 修饰 v-on。例如：

```
<my-component v-on:click.native="doTheThing"></my-component>
```


----------
**使用插槽分发内容**在使用组件时，我们常常要像这样组合它们：

```
<app>
  <app-header></app-header>
  <app-footer></app-footer>
</app>
```

注意两点：
`<app>` 组件不知道它会收到什么内容。这是由使用 `<app>` 的父组件决定的。
`<app>` 组件很可能有它自己的模板。
为了让组件可以组合，我们需要一种方式来混合父组件的内容与子组件自己的模板。这个过程被称为内容分发 (或“transclusion”如果你熟悉 Angular)。Vue.js 实现了一个内容分发 API，参照了当前 Web 组件规范草案，使用特殊的 `<slot>` 元素作为原始内容的插槽。
编译作用域

在深入内容分发 API 之前，我们先明确内容在哪个作用域里编译。假定模板为：

```
<child-component>
  {{ message }}
</child-component>
```

message 应该绑定到父组件的数据，还是绑定到子组件的数据？答案是父组件。组件作用域简单地说是：
父组件模板的内容在父组件作用域内编译；子组件模板的内容在子组件作用域内编译。
一个常见错误是试图在父组件模板内将一个指令绑定到子组件的属性/方法：

```
<!-- 无效 -->
<child-component v-show="someChildProperty"></child-component>
```

假定 someChildProperty 是子组件的属性，上例不会如预期那样工作。父组件模板不应该知道子组件的状态。
如果要绑定作用域内的指令到一个组件的根节点，你应当在组件自己的模板上做：

```
Vue.component('child-component', {
  // 有效，因为是在正确的作用域内
  template: '<div v-show="someChildProperty">Child</div>',
  data: function () {
    return {
      someChildProperty: true
    }
  }
})
```

类似地，分发内容是在父作用域内编译。
单个插槽

除非子组件模板包含至少一个 `<slot>` 插口，否则父组件的内容将会被丢弃。当子组件模板只有一个没有属性的插槽时，父组件整个内容片段将插入到插槽所在的 DOM 位置，并替换掉插槽标签本身。
最初在 `<slot>` 标签中的任何内容都被视为备用内容。备用内容在子组件的作用域内编译，并且只有在宿主元素为空，且没有要插入的内容时才显示备用内容。
假定 my-component 组件有下面模板：

```
<div>
  <h2>我是子组件的标题</h2>
  <slot>
    只有在没有要分发的内容时才会显示。
  </slot>
</div>
```

父组件模板：

```
<div>
  <h1>我是父组件的标题</h1>
  <my-component>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </my-component>
</div>
```

渲染结果：

```
<div>
  <h1>我是父组件的标题</h1>
  <div>
    <h2>我是子组件的标题</h2>
    <p>这是一些初始内容</p>
    <p>这是更多的初始内容</p>
  </div>
</div>
```

具名插槽

`<slot>` 元素可以用一个特殊的属性 name 来配置如何分发内容。多个插槽可以有不同的名字。具名插槽将匹配内容片段中有对应 slot 特性的元素。
仍然可以有一个匿名插槽，它是默认插槽，作为找不到匹配的内容片段的备用插槽。如果没有默认插槽，这些找不到匹配的内容片段将被抛弃。
例如，假定我们有一个 app-layout 组件，它的模板为：

```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

父组件模板：

```
<app-layout>
  <h1 slot="header">这里可能是一个页面标题</h1>
  <p>主要内容的一个段落。</p>
  <p>另一个主要段落。</p>
  <p slot="footer">这里有一些联系信息</p>
</app-layout>
```

渲染结果为：

```
<div class="container">
  <header>
    <h1>这里可能是一个页面标题</h1>
  </header>
  <main>
    <p>主要内容的一个段落。</p>
    <p>另一个主要段落。</p>
  </main>
  <footer>
    <p>这里有一些联系信息</p>
  </footer>
</div>
```

在组合组件时，内容分发 API 是非常有用的机制。


----------


**作用域插槽**
作用域插槽是一种特殊类型的插槽，用作使用一个 (能够传递数据到) 可重用模板替换已渲染元素。
在子组件中，只需将数据传递到插槽，就像你将 props 传递给组件一样：

```
<div class="child">
  <slot text="hello from child"></slot>
</div>
```

在父级中，具有特殊属性 scope 的 <template> 元素必须存在，表示它是作用域插槽的模板。scope 的值对应一个临时变量名，此变量接收从子组件中传递的 props 对象：

```
<div class="parent">
  <child>
    <template scope="props">
      <span>hello from parent</span>
      <span>{{ props.text }}</span>
    </template>
  </child>
</div>
```

如果我们渲染以上结果，得到的输出会是：

```
<div class="parent">
  <div class="child">
    <span>hello from parent</span>
    <span>hello from child</span>
  </div>
</div>
```

作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项：

```
<my-awesome-list :items="items">
  <!-- 作用域插槽也可以是具名的 -->
  <template slot="item" scope="props">
    <li class="my-fancy-item">{{ props.text }}</li>
  </template>
</my-awesome-list>
```

列表组件的模板：

```
<ul>
  <slot name="item"
    v-for="item in items"
    :text="item.text">
    <!-- 这里写入备用内容 -->
  </slot>
</ul>
```

----------
**动态组件**
使用is动态绑定component
**keep-alive**
如果把切换出去的组件保留在内存中，可以保留它的状态或避免重新渲染。为此可以添加一个 keep-alive 指令参数：

```
<keep-alive>
  <component :is="currentView">
    <!-- 非活动组件将被缓存！ -->
  </component>
</keep-alive>
```

异步组件

在大型应用中，我们可能需要将应用拆分为多个小模块，按需从服务器下载。为了让事情更简单，Vue.js 允许将组件定义为一个工厂函数，动态地解析组件的定义。Vue.js 只在组件需要渲染时触发工厂函数，并且把结果缓存起来，用于后面的再次渲染。例如：

```
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // Pass the component definition to the resolve callback
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

工厂函数接收一个 resolve 回调，在收到从服务器下载的组件定义时调用。也可以调用 reject(reason) 指示加载失败。这里 setTimeout 只是为了演示。怎么获取组件完全由你决定。推荐配合使用 ：Webpack 的代码分割功能：

```
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 require 语法告诉 webpack
  // 自动将编译后的代码分割成不同的块，
  // 这些块将通过 Ajax 请求自动下载。
  require(['./my-async-component'], resolve)
})
```

你可以使用 Webpack 2 + ES2015 的语法返回一个 Promise resolve 函数：

```
Vue.component(
  'async-webpack-example',
  () => import('./my-async-component')
)
```

当使用局部注册时，你也可以直接提供一个返回 Promise 的函数：

```
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```