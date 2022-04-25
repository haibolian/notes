[TOC]



### 1. 认识路由

路由中有一个非常重要的概念叫路由表，路由表本质上就是一个映射表, 决定了数据包的指向。

#### 1.1后端路由阶段

早期的网站开发整个HTML页面是由服务器来渲染的，服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示。这种情况就是**后端渲染**，也叫做服务端渲染。

但是, 一个网站服务器如何处理呢?

一个页面有自己对应的, 这么多页面网址, 也就是URL。

URL会发送到服务器，服务器会通过正则对该URL进行匹配，并且最后交给一个Controller进行处理。

Controller进行各种处理, ，终生成HTML或者数据, 返回给前端。

上面这种操作，就是**后端路由**，也就是后端处理URL和页面之间的映射关系。

| 浏览器                          |                   服务器                    |
| :------------------------------ | :-----------------------------------------: |
| http://www.taobao.com           | 1. 解析url 2. 将url对应的网页内容传给浏览器 |
| http://www.taobao.com/nanzhaung | 1. 解析url 2. 将url对应的网页内容传给浏览器 |

当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户端。

这种情况下渲染好的页面, 不需要单独加载任何的js和css, 可以直接交给浏览器展示, 这样也有利于SEO的优化。

但后端路由也有很大的缺点：

一种情况是整个页面的模块由后端人员来编写和维护的；

另一种情况是前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码；

而且通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情。

#### 1.2前后端分离阶段

随着Ajax的出现, 有了前后端分离的开发模式。

服务器那边有**静态资源服务器**和负责**提供API接口的服务器**。当用户输入网址进入网页时，是从静态资源服务器获取的html+css+js代码，并通过浏览器渲染到页面中，当用户发送ajax请求时，后端只提供API来返回数据, 前端通过Ajax获取数据, 并且可以通过JavaScript将数据渲染到页面中。

这种情况就叫做**前端渲染**，因为浏览器中显示的网页的大部分内容，都是由q前端写的js代码在浏览器中执行来渲染出来的网页。

这样做最大的优点就是前后端责任的清晰, 后端专注于数据上, 前端专注于交互和可视化上。

并且当移动端(iOS/Android)出现后, 后端不需要进行任何处理, 依然使用之前的一套API即可。

目前很多的网站依然采用这种模式开发。

#### 1.3单页面富应用(SPA)阶段

其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由。

整个网页只有一个html页面。

也就是前端来维护一套路由规则。

浏览器首次访问地址的时候会从服务器将全部的html+css+JavaScript全部资源下载下来，当点击其他按钮改变url的时候，这个时候url 不会向服务器请求其他资源，而是通过js代码的判断要显示的一些内容。

| 浏览器                     | 服务器                     |
| -------------------------- | -------------------------- |
| http://haibolian.com       | 将html+css+js 发送给浏览器 |
| http://haibolian.com/about |                            |
| http://haibolian.com/mine  |                            |

url变化时，会从已经下载下来的html+css+js中抽取相关的资源渲染到页面中，并不会请求服务器。

像这样由前端来管理 url 和页面的映射关系，就是**前端路由**。

那么，如何实现改变url而不让页面整体刷新呢？

有两种方案，一种是url 的hash ，一种是html5 的history

#### 1.4 hash 和 history

**hash**

URL的hash也就是锚点(#), 本质上是改变window.location的href属性。可以通过直接赋值location.hash来改变href, 但是页面不发生刷新。

```
>location.href
 "http://localhost:8080/"
>location.hash='/home'
 "/home"
>location.hash='/about'
 "/about"
```

**history**

history接口是HTML5新增的, 它有五种模式改变URL而不刷新页面.

- history.pushState()

```
>history.pushState({},'','/foo')
 undefined
>location.href
 "http://localhost:8080/foo"
>history.pushState({},'','/')
 undefined
```

pushState() 是以栈的形式进行url 的改变的

- history.replaceState()

history.replaceState() 是将当前的url 改变为 要改的url

```
>history.replaceState({},'','/foo')
 undefined
```

- history.go(-1)

```
>history.pushState({},'','/home')
 undefined
>history.go(-1)
 undefined
```

- history.back()等价于 history.go(-1) 

-  history.forward() 等价于 history.go(1) 

### 2. vue-router基本使用

vue-router官方文档：<a href="https://router.vuejs.org/zh/" style="text-decoration: none">Vue Router</a>

vue-router是Vue.js官方的路由插件，它和vue.js是深度集成的，适合用于构建单页面应用。

#### 2.1 安装

```
npm install vue-router --save
```

在src/router/index.js 下配置路由相关信息。

第一步：导入路由对象，并且调用 Vue.use(VueRouter)

```javascript
//vue-router是一个vue插件，任何插件都需要使用Vue.use(插件)来安装插件
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

第二步：创建路由实例，并且传入路由映射配置

```javascript
const routes=[

]

const router = new VueRouter({
  routes
})

export default router
```

第三步：在Vue实例中挂载创建的路由实例

```javascript
// main.js
import router from './router/index'
```

#### 2.2 使用

第一步: 创建路由组件

```html
<!-- Home.vue --> 
<template>
  <div>
    <h2>我是主页内容</h2>
  </div>
</template>
<script>
```

```html
<!-- About.vue -->
<template>
  <div>
    <h2>我是关于页</h2>
  </div>
</template>
```

第二步: 配置路由映射: 组件和路径映射关系

```javascript
// src/router/index.js
const routes=[
  {
    path: '/home',
    component: Home
  },
  {
    path: '/about',
    component: About
  }
]
```

第三步: 使用路由: 通过\<router-link>和\<router-view>

```html
<!--App.vue -->
<template>
  <div id="app">
    <router-link to="/home">首页</router-link>
    <router-link to="/about">关于</router-link>
    <router-view></router-view>
  </div>
</template>
```

\<router-view>用来显示组件内容，相当于组件的占位符，\<router-view>的位置决定了组件内容的显示位置。

第四步：挂载根实例

```js
const app = new Vue({
  router
}).$mount('#app')	
```

> 接下来，就可以在任何组件内通过 `this.$router` 访问路由器，也可以通过 `this.$route` 访问当前路由。
>
> 当 \<router-link>对应的路由匹配成功，将自动设置 class 属性值 `.router-link-active`。

#### 2.3 重定向

当第一次进入网站时，应该进入的是首页，但默认没有显示首页的组件，用户必须点击才可以进入。那么如何让用户已进入就可以进入首页呢，很简单，使用重定向让根路径指向home路径。

在routes 中新增一个映射即可：

```javascript
const routes=[
  {
    path: '/',
    redirect: '/home'
  },
```

当然还有一种方式是直接将路径  path：'/'  的component 设置为Home。但这样的话url 是不会显示当前位于/home路径的。

#### 2.4 改变路径方式

改变路径的方式有两种，分别为 URL 的 hash 和 html5 的 history 。默认情况下, 路径的改变使用的URL的hash。

如果希望使用html5 的history 模式，只需在router实例中添加mode: ’ history ‘

```javascript
const router = new VueRouter({
  routes,
  mode: 'history'
})
```

#### 2.5 router-link

router-link 中除了有可以跳转路径的 to 属性外，还有一些其他属性。

- **tag**: 

  tag可以指定`<router-link>`之后渲染成什么组件, 比如上面的代码会被渲染成一个`<li>`元素, 而不是`<a>`

- **replace**

  preplace不会留下history记录, 所以指定replace的情况下, 后退键返回不能返回到上一个页面中

- **active-class**

  当`<router-link>`对应的路由匹配成功时, 会自动给当前元素设置一个 `router-link-active` 的class, 在 router 实例中设置 `linkActiveClass` 可以自定义 `router-link-active`的名称。

  ```javascript
  const router = new VueRouter({
    routes,
    mode: 'history',
    linkActiveClass:'active'
  })
  ```

  这样当前元素的router-link-active类的名称会改为active。 通常用不着这样。

#### 2.6 路由代码的跳转

有时候, 页面的跳转可能需要执行对应的JavaScript代码, 这个时候, 就可以使用第二种跳转方式了。

比如将代码修改如下:

```html
<button @click='linktohome'>首页</button>
<button @click='linktoabout'>关于</button>
```

此时，相关的 js 跳转代码如下：

```javascript
name:'app',
methods: {
  linktohome(){
    this.$router.push('/home')
  },
  linktoabout(){
    this.$router.push('/about')
  }
},
```

#### 2.7 动态路由

在某些情况下，一个页面的path路径可能是不确定的，比如我们进入用户界面时，希望是如下的路径：/user/aaaa或/user/bbbb。

除了有前面的/user之外，后面还跟上了用户的ID。

这种path和Component的匹配关系，称之为动态路由(也是路由传递数据的一种方式)。

第一步：在路由实例中添加新的路径

```javascript
// router/index.js
{
  path: '/user/:id',
  component: User
}
```

第二步：在组件中使用$route.params.id 获取传来的 id 数据

```html
<!--User.vue -->
<template>
  <div>
    ID：{{ $route.params.id }}
  </div>
</template>
```

第三步：在`<router-link>`中传入数据

```html
<!--App.vue -->
<router-link to="/user/wang">用户</router-link>
```

浏览器中将显示：  ID：wang

当然，也可以使用 v-bind:to=" '/user/'+message"动态的传入数据。

#### 2.8 路由的懒加载

当打包构建应用时，Javascript 包会变得非常大，影响页面加载。

如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

路由懒加载使用方法：

```javascript
// 使用路由懒加载后则不需要在这里导入组件了，删掉即可
// import Home from '../views/Home.vue'
// import About from '../views/About.vue'
// import User from '../views/User.vue'
const routes=[
  {
    path: '/home',
    // 这里是直接使用的，也可以将箭头函数赋值给变量，在此处引用变量
    component: ()=> import('../views/Home.vue')
  },
  {
    path: '/about',
    component: ()=> import('../views/About.vue')
  }
```

这只是路由懒加载的ES6方式，还有另外两种方式，日后涉及到的话再进行区别总结。

路由懒加载做了什么?

路由懒加载的主要作用就是将路由对应的组件打包成一个个的js代码块。

只有在这个路由被访问到的时候, 才加载对应的组件。

### 3. vue-router嵌套路由

比如在home页面中, 我们希望通过/home/news和/home/message访问一些内容。

| 路径          | 组件    |
| ------------- | ------- |
| /home         | Home    |
| /home/news    | News    |
| /home/message | Message |
| /about        | About   |

**实现步骤**

步骤一：创建对应的子组件, 并且在路由映射中配置对应的子路由

```html
<!-- News.vue -->
<template>
  <div
      <h2>新闻</h2>
  </div>
</template>
```

```html
<!-- Message.vue -->
<template>
  <div>
      <h2>消息</h2>
  </div>
</template>
```

```javascript
// router/index.js
const routes=[
  {path: '/home', component: ()=> import('../views/Home.vue'),
   // 使用children配置子路由
    children:[
      {
   	   path:'/home/news',
       component:()=>import('../components/home/News.vue')
	  },
      {
       path:'/home/message',
       component:()=>import('../components/home/Message.vue')
      }
    ]
  },
```

步骤二：在组件内部使用<router-view>标签.

```html
<!--Home.vue-->
<template>
  <div>
    <h2>我是主页内容</h2>
    <router-link to="/home/news">新闻</router-link>
    <router-link to="/home/message">消息</router-link>
    <router-view></router-view>
  </div>
</template>
```

**嵌套路由默认路径**

嵌套路由也可以使用默认路径，配置方式如下

```javascript
children:[
  // 这里的redirect不能带斜杠(/),如果带斜杠需要写成 /home/news
  {path:'',redirect:'news'},
  {path:'/home/news',component:()=>import('../components/home/News.vue')},
  {path:'/home/message',component:()=>import('../components/home/Message.vue')}
```

### 4. vue-router参数传递

传递参数主要有两种类型: params和query

**params 类型**

这种类型在2.7 动态路由中已经介绍，在此不多赘述。

**query 类型**

步骤一：普通配置路由格式即可

```javascript
{path: '/about', component: ()=> import('../views/About.vue')},
```

步骤二：传递的方式: 对象中使用query的key作为传递方式

```html
<router-link :to="{path:'/about',query:{name:'hehe',age:16}}">关于</router-link>
```

步骤二：传递的方式：通过JavaScript代码进行传递

```html
<button @click="toAbout">关于</button>
<script>
export default {
methods: {
  toAbout(){
    this.$router.push({
      path: '/about',
      query: { name: 'hehe',age: 9}
    })}},
```

步骤三：获取参数

```html
<template>
  <div>
    <h2>{{ $route.query }}</h2>
  </div>
</template>
```

### 5. vue-router导航守卫

在一个SPA应用中, 如何改变网页的标题呢?

网页标题是通过 `<title>` 来显示的, 但是SPA只有一个固定的HTML, 切换不同的页面时, 标题并不会改变。

但是我们可以通过JavaScript来修改`<title>`的内容.window.document.title = '新的标题'。

**普通的修改方式**

比较容易想到的修改标题的位置是每一个路由对应的组件.vue文件中，通过mounted声明周期函数, 执行对应的代码进行修改即可。但是当页面比较多时, 这种方式不容易维护。

**导航守卫**

> 记住**参数或查询的改变并不会触发进入/离开的导航守卫**。你可以通过[观察 `$route` 对象](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化)来应对这些变化，或使用 `beforeRouteUpdate` 的组件内守卫

vue-router提供的导航守卫主要用来监听监听路由的进入和离开的。

vue-router提供了beforeEach和afterEach的钩子函数, 它们会在路由即将改变前和改变后触发。

我们可以利用beforeEach来完成标题的修改。

首先, 我们可以在钩子当中定义一些标题, 可以利用meta来定义。

```javascript
onst routes=[
  {path: '/', redirect: '/home'},
  {path: '/home', component: Home,meta:{ title: '首页'},},
  {path: '/about', component: About,meta:{ title: '关于'}},
  {path: '/user/:id', component: User,meta:{ title: '用户'}} 
]
```

然后, 利用导航守卫,修改标题。

```javascript
const router = new VueRouter({
  routes,
  mode: 'history',
})
router.beforeEach((to,from,next)=>{
  window.document.title = to.meta.title;
  next()
})
```

上面这样使用的导航守卫，被称之为**全局守卫**。

- 上面的`to.meta.title`只适用于没有子路由的情况，比如/home下面还有news 和 message，如/home/news 或者/home/message，则/home的标题会显示 undefined，因为并没有给子路由添加 `meta:{title:'首页'}`，所以当进入/home/news 时，to.meta 为空。

  当子路由的`to.meta`为空时，console.log(to)后，打印出来的matched[0]中含有`meta: {title: "首页"}`。

  因此当有子路由的时候，可以使用如下代码来获取 title，这样不必为子路由添加 meta。

  ```javascript
  router.beforeEach((to,from,next)=>{
    window.document.title = to.matched[0].meta.title;
    next()
  })
  ```

- 导航钩子的三个参数分析：

  to: 即将要进入的目标的路由对象。

  from: 当前导航即将要离开的路由对象。

  next: 调用该方法后才能进入下一个钩子。

对于next() 方法，如果是后置钩子，也就是afterEach，不需要主动调用next() 函数。

next() 的参数中传入路径，如 `next('/')`或者`next({ path: '/' })`，则跳转到一个不同的地址。

**路由独享的守卫**

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
          // ....
          next()
      }
    }
  ]
})
```

可以看到，前置守卫钩子定义在路由中，我们可以在`beforeEnter`中定义一些进入该组件之前要做的一些事情。

**组件内的守卫**

- `beforeRouteEnter`
- `beforeRouteUpdate` 
- `beforeRouteLeave`

需求：进入首页，默认进入新闻界面(/home/news)，点击进入消息界面(/home/message)后进入关于页面(/about)。现在想要点击进入/home页面后，显示刚才离开时的内容( 即/home/message )。

在Home组件中定义组件内守卫：

```javascript
// Home.vue
export default {
  name:'home',
  data() {
    return {
      homePath:'/home/news'
    }
  },
  activated(){
    this.$router.push(this.homePath);
  },
  beforeRouteLeave(to, from, next) {
    this.homePath = this.$route.path;
    next();
  }
```

在离开Home组件时触发beforRouteLeave() 函数，记录离开前的url

activated() 函数是当组件处于活跃状态时。此时，进入刚才离开的url

**遇到的问题：**

我用的vue router 版本：3.1.3

控制台报错`message: "Navigating to current location ("/home/news") is not allowed"`

原因是，在路由跳转的时候两次push的path地址相同。

解决方法有两种：
1.切换版本回3.0版本。

2.在router/index.js文件里加上如下代码

```javascript
const originalPush = VueRouter.prototype.push
VueRouter.prototype.push = function push(location) {
  return originalPush.call(this, location).catch(err => err)
}
```

更多关于导航守卫知识见官网：<a href="https://router.vuejs.org/zh/guide/advanced/navigation-guards.html" style="text-decoration: none">导航守卫</a>

### 6. keep-alive

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

它们有两个非常重要的属性:

include - 字符串或正则表达，只有匹配的组件会被缓存。

exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存。

router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存：

```html
<keep-alive>
  <!-- 所有路径匹配到的视图组件都会被缓存 -->
  <router-view></router-view>
</keep-alive>
```

### 7. $route和$router

$route和$router是有区别的

$router为VueRouter实例，想要导航到不同URL，则使用$router.push方法。

$route为当前router跳转对象，里面可以获取name、path、query、params等 。

 

**更多 Vue-Router 内容见官网 [Vue-Router](https://router.vuejs.org/zh/)**