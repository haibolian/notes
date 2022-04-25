[TOC]



### 1. 状态管理

> Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

**状态管理模式、集中式存储管理**，其实可以简单的将其看成把需要多个组件共享的变量全部存储在一个对象里面。

然后，将这个对象放在顶层的Vue实例中，让其他组件可以使用。

那么，多个组件就可以共享这个对象中的所有变量属性了。

虽然我们自己也可以通过对象原型实现这一功能，但还需要做到响应式。尽管做到响应式，但封装起来还是比较麻烦的。

所以Vuex就是为了提供这样一个在多个组件间共享状态的插件。

#### 1.1 单界面的状态管理

<img src="https://gitee.com/haibolian/screenshot/raw/master/vue/单界面的状态管理.png" style="zoom: 54%;" />

其中，State 就可以看作是data中的数据，当执行各种操作时，data中的数据发生变化并显示在视图中。

#### 1.2 多界面状态管理

那么，多个视图都依赖同一个状态呢？不同界面的Actions都想修改同一个状态呢？该状态应该放在哪里合适？-----------**Vuex**

<img src="https://gitee.com/haibolian/screenshot/raw/master/vue/vuex.png" style="zoom: 80%;" />

#### 1.3 Vuex的使用

1. 在main.js中，导入store对象，并放入new Vue 中，这样，在其他Vue组件中，我们就可以通过this.$store的方式，获取到这个store对象了。

```javascript
import store from './store'
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```

2. 在其他组件中使用store对象中保存的状态

   通过this.$store.state.属性的方式来访问状态

   通过this.$store.commit('mutation中方法')来修改状态

注意：要通过提交mutation的方式，而非直接改变store.state.count。

这是因为Vuex可以更明确的追踪状态的变化，所以不要直接改变 store.state 中的值。

### 2. State

单一状态树英文名称是Single Source of Truth，也可以翻译成单一数据源。

如果状态信息是保存到多个Store对象中的，那么之后的管理和维护等等都会变得特别困难。

所以Vuex也使用了单一状态树来管理应用层级的全部状态。

单一状态树能够让我们最直接的方式找到某个状态的片段，而且在之后的维护和调试过程中，也可以非常方便的管理和维护。

### 3. Getters

getters 和 vue实例中的计算属性类似。

**Getters的第一个参数**

getters的第一个参数是state，固定的

```js
state: {
  stu:[
    { name:'唐僧',age:18 },
    { name:'孙悟空',age:21},
    { name:'猪八戒',age:15},
    { name:'沙僧',age:32 }
  ]
},
getters:{
    // 筛选age大于20的内容
  moreTwentyAge(state){
    return state.stu.filter(s=>s.age>20)
  }
```

**Getters的第二个参数**

getters的第一个参数是getters，固定的

```js
// age大于20的个数
morelegnth(state,getters){
  return getters.moreage.length;
}
```

**给 getters 传递参数**

getters第一第二个参数是固定的state和getters本身，没有第三个参数了，若想要给getters传递参数，则使 getters 返回一个函数，该函数包含参数。如下

```js
getContent(state){
  return function(age){
    return state.stu.filter(s=>s.age>age)
  }
}
```

这样一来，当别的组件调用getters的 getContent 属性并传递指定参数时，即可将内容返回。

### 4. Mutation

Vuex的store状态的更新唯一方式：**提交Mutation**

#### 4.1 Mutation基本使用

Mutation主要包括两部分：

字符串的**事件类型（type）**

一个**回调函数（handler）**,该回调函数的第一个参数就是state。

mutation定义方式：

```javascript
mutations: {
  increment(state){
    return state.count++;
  }
```

组件通过commit通知store更新

```javascript
methods: {
  countAdd(){
    this.$store.commit('increment')
  }
```

#### 4.2 Mutation传递参数

在通过mutation更新数据的时候, 有可能我们希望携带一些**额外的参数**

参数被称为是mutation的载荷(Payload)，看如下代码

```javascript
// vuex/index.js
mutations: {
  increment(state,n){
    state.count = state.count +n;
  }
```

```javascript
// .vue文件
methods: {
  countAdd(){
    this.$store.commit('increment',10086)
  }
```

这是传递一个参数，那如果不是一个呢？

这时通常以对象的形式传递，也就是payload

```javascript
// vuex/index.js
increment(state,payload){
  state.count = state.count +payload.aaa;
}
```

```javascript
// .vue文件
countAdd(){
  this.$store.commit('increment',{ aaa:5,bbb:10 })
}
```

#### 4.3 Mutation提交风格

上面通过commit进行提交是一种普通的方式，Vuex 还提供了另一种风格，包含type属性的对象。

```javascript
// .vue
methods: {
  countAdd(){
    this.$store.commit({
      type:'increment',
      aaa:5
    })
```

Mutation中的处理方式是将整个commit的对象作为payload使用, 所以vuex/index.js中的代码没有改变。

#### 4.4 Mutation相应规则

Vuex的store中的state是响应式的, 当state中的数据发生改变时, Vue组件会自动更新。

这就要求我们必须遵守一些Vuex对应的规则：

- 提前在store中初始化好所需的属性

- 当给state中的对象添加新属性时, 使用下面的方式

  方式一: 使用Vue.set(state.stu, 'newProp', 123)

  方式二: 用新对象给旧对象重新赋值，看例子

  ```javascript
  changInfo(state){
    state.count = { ...state.info,'bbb':'bbb' } 
  }
  ```

- 对于数组，可参考 [**Vue(一)基础**](https://haibolian.github.io/2019/07/16/Vue(一)基础/)篇中的 **5.6 循环遍历**这一节中的**检测数组更新**内容。

直接给对象赋新的属性和值的方式不能做到响应式，如 `obj.props = ' aaa '`，虽然state中的obj确实有了props这个属性，但视图中没有显示。

#### 4.5 Mutation同步函数

通常情况下, Vuex要求我们Mutation中的方法必须是同步方法。

主要的原因是当我们使用devtools时, 可以devtools可以帮助我们捕捉mutation的快照。

但是如果是异步操作, 那么devtools将不能很好的追踪这个操作什么时候会被完成。

所以，通常情况下,不要在mutation中进行异步的操作。

### 5. Action

Action类似于Mutation, 但是是用来代替Mutation进行异步操作的。

```javascript
// vuex/index.js
mutations: {
  increment(state,payload){
    state.count = state.count +payload.aaa;
  }
},
actions: {
  increment(context,payload){
    setTimeout(()=>{
      context.commit('increment',payload)
    },3000)
  }
```

```javascript
// .vue
getAction(){
  this.$store.dispatch('increment',{aaa:10})
}
```

在store中，actions 中的方法第一个参数是 context，context是和store对象具有相同方法和属性的对象。

也就是说, 我们可以通过context去进行commit相关的操作, 也可以获取context.state等.

但是注意，这里它们并不是同一个对象。

在.vue 组件中，方法通过dispatch() 去调用 store 的 actions 里面的方法执行异步操作。当然也可以传递参数到 payload中。

**Action返回的Promise**

在Action中, 我们可以将异步操作放在一个Promise中, 并且在成功或者失败后, 调用对应的resolve或reject。

```javascript
actions: {
  increment(context,payload){
    return new Promise((resolve)=>{
      setTimeout(()=>{
        context.commit('increment',payload)
        resolve()
      },1000)
    })
```

```javascript
getAction(){
  this.$store.dispatch('increment',{aaa:10}).then((res)=>{
    console.log('执行异步操作成功');
  })
}
```

在action中的方法里面通过返回一个promise，并传入resolve参数和执行resolve参数，这样在vue组件中即可使用 .then 来执行异步操作的回调函数。

### 6. Module

Vue使用单一状态树,那么也意味着很多状态都会交给Vuex来管理。

当应用变得非常复杂时，store对象就有可能变得相当臃肿。

为了解决这个问题, Vuex允许我们将store分割成模块(Module)，而每个模块拥有自己的state、mutations、actions、getters等。

```javascript
export default new Vuex.Store({
  state: {...},
  getters: {...},
  mutations: {...},
  actions: {...},
  modules: {
    a:{
        state:{...},
        getters: {...},
  		mutations: {...},
  		actions: {... },
    }
    b:{
        state:{...},
        getters: {...},
  		mutations: {...},
  		actions: {... },
    }
  }
})

```

上面这种形式书写并不好，应将modules中的模块抽离出来，如下，这样就可以更好的管理各个modules了。

```javascript
const moduleA = {
  state:{
      name:'alan'
  },
  getters:{},
  mutations:{},
  actions:{}
}
const moduleB = {
  state:{},
  getters:{},
  mutations:{},
  actions:{}
}
export default new Vuex.Store({
  state: {...},
  getters: {...},
  mutations: {...},
  actions: {...},
  modules: {
    a: moduleA,
    b: moduleB
  }
})
```

#### 6.1 获取module中的state

那么，如何获取模块中的state呢？

获取方式有点奇怪，并不是`$store.modules.a.state.name`或者`$store.state.name`。

因为 vue 会将modules 中的模块放到 state 中作为对象。

所以获取模块中的state方法是 `$store.state.a.name`这种方式。

#### 6.2 使用module中的mutations

modules中的mutations的名字要和store中的区分开来，不能写同样的。

```javascript
export default new Vuex.Store({
  state:{ age:0 }
  mutations: {
    increments(state){
      state.age++
    }
  },
  modules: {
    a: moduleA,
    b: moduleB
  }
})
const moduleA = {
  state:{
    name:'zhangsan',
    count:100
  },
  mutations:{
    increment(state){
      state.count++
    }
}
```

```javascript
<button @click="increment">按钮</button>
{{ $store.state.count }}   // 0++
{{ $store.state.a.count }} // 100++
export default {
methods: {
    increment(){
      this.$store.commit('increment')
    }
},
```

如上代码，store和 moduleA中都含有相同的increment，当vue组件中想要执行moduleA中的increment时，store中的count和moduleA中的age都会自增（按道理来说不应该是其中只有一个自增吗？）。

因此 modules 中的mutations的名称要和 store中mutations的名称区分开来。

那么vue组件要想执行 modules 中的 mutations，则直接和执行 store中的mutations方式一样。即`this.$store.commit('方法名')`

#### 6.2 使用module中的getters

modules中的getters的名字要和store中的getters区分开来，不能写同样的。

和 mutations 差不多，如果moduels 中getters的名字和store 中的getters名称相同的话，vue组件在获取的时候获取的是store中的getters，并且会报错，错误内容是： `duplicate getter key: fullname`即重复的fullname（fullname是我定义的一个getters）。

因此，在命名时，应尽量避免同名。

modules中的getters的第一个参数state和第二个参数getters和在store中相同，都是当前对象中的state和getters。

但模块中的getters含有第三个参数 rootState 和第四个参数rootGetters。顾名思义，第三个参数是store中的state，第四个参数时store 中的getters

当modules中的getters想要获取store中的state时或者getters时，可启用第三个或第四个参数。

```js
const moduleA = {
  state:{
    name:'zhangsan',
    age:100
  },
  getters:{
    moduleAge(state){
      return state.age-82
    },
    fullname(state,getters,rootState,rootGetters){
      return state.name+'的年龄为'+getters.moduleAge+
      rootState.name+'的年龄为'+rootGetters.lisiAge
    }
  }
}
```

#### 6.3 Actions的写法

对于模块内部的 action，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`：

```js
const moduleA = {
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

#### 6.4 命名空间

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。

```js
const moduleA = {
  namespaced: true,
  state:{
  },
  getters:{
  }
}
```

使用命名空间后，modules中的名称可以和store中的名称相同。

那么，**使用命名空间后**，state可以通过`$store.state.a.count`这种形式获取，那**getters、mutations和actions如何获取呢**？

刚开始没有仔细阅读官网注释内容，在vue-devtools工具中看到 **6.2**这一节代码中的fullname在getters/a/fullname中，所以打算用`$store.getters.a.fullname`来获取该值，但是报错说找不到未定义的fullname，很不解。

再看官网中代码的注释

```js
 getters: {
   isAdmin () { ... } // -> getters['account/isAdmin']
 },
 actions: {
   login () { ... } // -> dispatch('account/login')
 },
 mutations: {
    login () { ... } // -> commit('account/login')
 },
```

所以，在组件中可以通过形如`$store.getters['a/fullname']`的方式来获取modules 中的getters。actions和mutations 参考上面代码注释即可。

```js
{{ $store.getters['a/fullname']}}
console.log(this.$store.getters['a/fullname']);
```



**更多关于Vuex内容见 [Vuex官网](https://vuex.vuejs.org/zh/guide/)**



