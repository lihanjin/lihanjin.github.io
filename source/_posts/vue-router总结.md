---
title: Vue-Router整理
date: 2017-09-12 22:24:08
tags: Vue
categories: Vue
---
##VUE-ROUTER API文档


----------


</br>
### 一.`<router-link>`
1.`Props`
- `to`
 - 类型:`string` | `Location`
 - `required`
表示目标路由的链接。当被点击后，内部会立刻把`to`的值传到`router.push()`，所以这个值是可以一个字符串或者是描述目标位置的对象。
```
 <!-- 字符串 -->
  <router-link to="home">Home</router-link>
  <!-- 渲染结果 -->
  <a href="home">Home</a>

  <!-- 使用 v-bind 的 JS 表达式 -->
  <router-link v-bind:to="'home'">Home</router-link>

  <!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
  <router-link :to="'home'">Home</router-link>

  <!-- 同上 -->
  <router-link :to="{ path: 'home' }">Home</router-link>

  <!-- 命名的路由 -->
  <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

  <!-- 带查询参数，下面的结果为 /register?plan=private -->
  <router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
```
`to`可以是字符串或者表达式，动态设置需要加上`v-bind`或：  `path`路径 `params`传参 `query`问号后面的值。
- `replace`
- 类型:`boolean`
- 默认值:`false`
设置了`replace`的属性话，当点击时，会调用`router.replace()`而不是`router.push()`
于是导航不会留下`history`记录。
`<router-link :to="{path: '/home'} replace></router-link>"`
- `append`
- 类型:`boolean`
- 默认值:`false`
设置`append`属性后，则在当前(相对)路径前添加基路径。例如，我们从/a导航到一个相对路径`b`，如果没有配置`append`，则路径为`/b`,如果赔了，则为`/a/b`
`<router-link :to="{path : "relative/path" append} </router-link>"`
- `tag`
	- 类型: `string`
	- 默认值: `"a"`
	有时候想要 `<router-link>` 渲染成某种标签，例如 `<li>`。 于是我们使用 tag prop 类指定何种标签，同样它还是会监听点击，触发导航。

```
  <router-link to="/foo" tag="li">foo</router-link>
  <!-- 渲染结果 -->
  <li>foo</li>
```
- `active-class`
- 类型:`string`
- 默认值:"`router-link-active`"
设置链接激活时使用的`css`类名。默认值可以通过路由的构造选项`linkActiveClass`来全局配置。
- `exact`
	- 类型:`boolean`
	- 默认值:`false`
"是否激活" 默认类名的依据是 `inclusive match` （全包含匹配）。 举个例子，如果当前的路径是 `/a` 开头的，那么 `<router-link to="/a">` 也会被设置 `CSS` 类名。

按照这个规则，`<router-link to="/">` 将会点亮各个路由！想要链接使用 "`exact` 匹配模式"，则使用 `exact` 属性：

```
  <!-- 这个链接只会在地址为 / 的时候被激活 -->
  <router-link to="/" exact>
```
- events
	- 类型: string | Array<string>
	- 默认值: 'click'
声明可以用来触发导航的时间。可以是一个字符串或是一个包含字符串的数组

#### **将"激活时的CSS类名"应用在外层元素**

有时候我们要让 "激活时的CSS类名" 应用在外层元素，而不是 `<a>` 标签本身，那么可以用 `<router-link>` 渲染外层元素，包裹着内层的原生 `<a>` 标签：

```
<router-link tag="li" to="/foo">
  <a>/foo</a>
</router-link>
```

在这种情况下，`<a>` 将作为真实的链接（它会获得正确的 `href` 的），而 "激活时的CSS类名" 则设置到外层的 
</br>
</br>


----------


### 二.`<router-view>`
`<router-view>`组件是一个functional组件，渲染路径匹配到的视图组件。`<router-view>`渲染的组件还可以内嵌自己的`<router-view>`，根据嵌套的路径，渲染嵌套组件。
#### 属性
- name 
	- 类型:string
	- 默认值:"default"
	如果`<router-view>`设置了名称，则会渲染对应的路由配置中comonents下的相应组件。查看命名视图中的列子。
#### 行为表现
其他属性（非router-view使用的属性）都直接传给渲染的组件，很多时候，每个路由的数据都是包含在路由的参数中。
因为它也是一个组件，可以配合`<transtion>`和`<keep-alive>`使用，如果两者结合使用一定要保证在内层使用`<keep-alive>`
```
<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>
```
</br>
</br>


----------


### 三.路由信息对象
一个route object（路由信息对象）表示当前激活的路由的状态信息，包含了当前的url解析得到的信息，还有url匹配到的route record （路由记录）
  
  route object是immutable（不可变）的，每次成功的导航后都会产生一个新的对象。
  
  route object出现在多个地方：
  - 组件内的`this.$route`和`$route watcher`回调（监测变化处理）；
  - `router.match(location)`的返回值
  - 导航钩子的参数:
```
  router.beforeEach((to,from,next)=>{
		//ro 和 from 都是路由信息对象
	})
```


----------


### 路由信息对象的属性
- $route.path
	- 类型:string
		字符串，对应当前的路径，总是解析为绝对路径，如`"/foo/bar"`。
- $route.params
	- 类型:Object
	一个key/value对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。
- $route.query
	- 类型：Object
	一个key/value对象，表示URL查询参数。例如，对于路径/foo?user=1,则有`$route.query.user=1`,如果没有查询参数，则是一个空对象。
- $route.hash
	- 类型:string
	当前路由的hash值（带#），如果没有hash值，则为空字符串。
- $route.fullPath
	- 类型:string
	完成解析后的URL，包含查询参数和hash的完整路径。
- $route.matched
	- 类型:`Array<RouteRecord>`
	一个数组，包含当前路由的所有嵌套路径片段的路由记录。路由记录就是routes配置数组中的对象副本（还有在children数组）。
	

```
const router = new VueRouter({
  routes: [
    // 下面的对象就是 route record
    { path: '/foo', component: Foo,
      children: [
        // 这也是个 route record
        { path: 'bar', component: Bar }
      ]
    }
  ]
})
```

当 URL 为 /foo/bar，`$route.matched` 将会是一个包含从上到下的所有对象（副本）。

- $route.name

当前路由的名称，如果有的话。（查看 命名路由）
![enter image description here](http://otue68nu2.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170919105103.png)
</br>
</br>


----------


### 四.Router构造配置
**routes**
- 类型`Array<RouteConfig>`
	RouteConfig的类型定义：
```
	declare type RouteConfig = {
  path: string;//路径
  component?: Component;//组件
  name?: string; // for named routes (命名路由)
  components?: { [name: string]: Component }; // for named views (命名视图组件)
  redirect?: string | Location | Function;//重定向
  alias?: string | Array<string>;//别名
  children?: Array<RouteConfig>; // for nested routes
  beforeEnter?: (to: Route, from: Route, next: Function) => void;//全局route注册之前的钩子
  meta?: any;//路由配置元信息meta字段
}
```
**mode**
- 类型:`string`
- 默认值:`"hash‘(浏览器环境) | "abstract"(Node.js环境)`
- 可选值:`"hash" | "history" | "abstract"`
	配置路由模式：
	- hash:使用URL hash值来做路由，支持所有浏览器，包括不支持HTML5 History Api的浏览器
	- history:依赖HTML5 History API和服务器配置，查看<a href="https://router.vuejs.org/zh-cn/essentials/history-mode.html">HTML5 History模式 </a>
	- abstract:支持所有JavaScript运行环境，如Node.js服务器端。如果发现没有浏览器的API,路由会自动强制进入这个模式
**base**
- 类型:string
- 默认值:"/"
应用的基路径。例如，如果整个单页面应用服务在`/app/`下，然后base就应该设为`"/app/"`。
**linkActiveClass**
- 类型:string
- 默认值:"router-link-active"
全局配置<router-link>的默认[激活cass类名]。参考router-link
**scrollBehavior**
- 类型:function
	签名：
```
	(
  to: Route,
  from: Route,
  savedPosition?: { x: number, y: number }
) => { x: number, y: number } | { selector: string } | ?{}
```

更多详情参考<a href="https://router.vuejs.org/zh-cn/advanced/scroll-behavior.html"> 滚动行为</a>.
</br>
</br>


----------


### 五.Router实例
**属性**
router.app
- 类型:Vue instace
	配置了router的Vue根实例。

route.mode
- 类型:string
	路由使用的<a href="https://router.vuejs.org/zh-cn/api/options.html#mode">模式</a>。 

router.currentRoute
- 类型:Route
当前路由对应的路由信息对象<a href="https://router.vuejs.org/zh-cn/api/route-object.html">信息对象</a>
**方法**
- router.beforeEach(guard)路由加载之前
- router.befireResolve(guard)此时异步组件已加载完成
- router.afterEach(hood)
	增加全局的导航钩子
- router.push(location,ONccomplete?,onAbort?)
- router.replace(location,ONccomplete?,onAbort?)
- router.go(n)
- router.back()
- router.forward()
动态的导航到一个新 url。
- router.getMatchedComponents(location?)

返回目标位置或是当前路由匹配的组件数组（是数组的定义/构造类，不是实例）。通常在服务端渲染的数据预加载时时候。

- router.resolve(location, current?, append?)
解析目标位置，返回包含如下属性的对象

```
{
  location: Location;
  route: Route;
  href: string;
}
```
- router.addRoutes(routes)


动态添加更多的路由规则。参数必须是一个符合 routes 选项要求的数组。

- router.onReady(callback)



添加一个会在第一次路由跳转完成时被调用的回调函数。此方法通常用于等待异步的导航钩子完成，比如在进行服务端渲染的时候。