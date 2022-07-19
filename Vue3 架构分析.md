# Vue3 项目分析



## 1. Vue3

### 优点：

* **（运行）性能比 vue2 快 1.2 ~ 2 倍**

  * 首先是 `Object.defineProperty` 转为 `proxy` 后，进入组件时，无需再为了给数据添加侦听器而递归遍历整个 data，减少了创建组件实例的性能开销。

    也不需要担心数组的响应式了。

  * 虚拟 dom 之间的比较优化，diff 算法优化，节约了不必要的性能消耗。

* **（运行）体积更小**

  * 移除一些 API， API 减少了，代码体积也随之减少了

  * 更利于 `tree-shaking` ，树摇，摇掉未被使用的代码，举个例子：

    ```js
    // 在 vue2 中
    import Vue from 'vue'
    
    Vue.nextTick(() => {
      // DOM相关操作
    })
    
    // 在 vue3 中
    import { nextTick } from 'vue'
    
    nextTick(() => {
      // DOM相关操作
    })
    ```

    如上代码，

    在 vue2 中打包时无论是否使用 `nextTick`，nextTick 都会被打包进去，还有比如 `Vue.set`

    在Vue3 中，`nextTick` 等许多内置 api 需要单独导入，如果没有用到 `nextTick` ，则不会进行打包

  * 按需编译，体积比Vue2更小。`import { computed, watch } from "vue";` 如果没有用到 `computed` 则不会将其打包

* **（开发）代码重用，更好的代码抽离** 

  **在 vue2 中**，**公共逻辑**我们可能会放到 `mixin` 中，但 `mixin` 会带来几个坏处：

  1. 当我们使用多个 `mixin` 时，我们并不清楚的知道这个 `data` 或 `method` 或 `computed`来自哪个 `mixin`，写预算时我有用到过几个 mixin

     ```vue
     export default {
     	mixins: [a, b, c]
     }
     ```

  2. 命名空间冲突

  3. 隐式跨 mixin 交流，多个 mixin 可能会互相作用，耦合性高

  **在 Vue3 中**，可以用组合式函数（hook），这也是 Vue3 中很重要的一个点，借鉴于 react 的 hooks， 逻辑关注点分离，对大型项目会更加友好，引个官网的例子：

  ```js
  // mouse.js
  import { ref, onMounted, onUnmounted } from 'vue'
  
  // 按照惯例，组合式函数名以“use”开头
  export function useMouse() {
    // 被组合式函数封装和管理的状态
    const x = ref(0)
    const y = ref(0)
  
    // 组合式函数可以随时更改其状态。
    function update(event) {
      x.value = event.pageX
      y.value = event.pageY
    }
  
    // 一个组合式函数也可以挂靠在所属组件的生命周期上
    // 来启动和卸载副作用
    onMounted(() => window.addEventListener('mousemove', update))
    onUnmounted(() => window.removeEventListener('mousemove', update))
  
    // 通过返回值暴露所管理的状态
    return { x, y }
  }
  ```

* **更好的组织代码，逻辑更清晰**

在 vue2 的 options-api 写法，我们要处理一些逻辑，要先在 data 里面定义好属性，然后这个属性可能在computed、methods、生命周期中操作，开发和维护时都需要跳来跳去。

在 vue3 的  composition-api 写法，逻辑可以写一块。尤其是对于复杂的业务逻辑，更利于开发和维护。如下图，不同颜色块代表不同的业务逻辑块。

![](https://fjolt.com/images/misc/08052022.webp)

![](https://img2020.cnblogs.com/blog/1069051/202103/1069051-20210319140832703-1591387698.png)

### 缺点

* **（运行）兼容性稍逊 Vue2，不支持 IE 浏览器**
* **（开发）增加学习成本（其实也算个优点，这种写法对项目开发和维护都有帮助）**



## 2. vite

### 优点

* （开发）快速启动，不需要等待打包，真正的按需编译，不用等待整个项目编译完成
* （开发）热更新比 weback 更快

### 缺点

* （运行）只针对支持 ES6 的浏览器
* （开发）新的 api 学习成本
* （开发）生态不如 webpack



## 3. element-plus

* **不需要担心虚拟表格了，element-plus 已经内置了这个组件，还增加了很多其他组件**

  [element-plus虚拟化表格](https://element-plus.gitee.io/zh-CN/component/table-v2.html#%E6%A0%91%E5%BD%A2%E6%95%B0%E6%8D%AE)，如果需要层级按钮组，需要自己添加。

* 其中还有**树形选择**和**虚拟化树形选择**，选 BU、门店那种。

  关于**树形选择**，我拿阿里的组织架构树试过，不需要虚拟化也能流畅展开。



## 4. TypeScript

### 优点

* **TypeScript 增加了代码的可读性和可维护性**

  - 类型系统实际上是最好的文档，大部分的函数看看类型的定义就可以知道如何使用了

  - TypeScript 要编译后才能使用。所以我们的**类型错误会在编译过程中被编译器发现**，更早发现 BUG。

    比如原本应该是两个数字相加的，写错成两个数字形式的字符串相加，此时编译器会报错

  - 增强了编辑器和 IDE 的功能，包括代码补全、代码提示、跳转到定义、重构等

* **兼容性好**

  * `.js` 文件可以直接重命名为 `.ts` 即可
  * 兼容第三方库，即使不是用ts编写的
  * 有活跃的社区，大多数的第三方库都可提供给 ts 的类型定义文件，完全支持 es6 规范

### 缺点

* 有一定的学习成本，需要理解接口（Interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）等前端工程师可能不是很熟悉的概念

- 短期可能会增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期维护的项目，TypeScript 能够减少其维护成本
- 一些冷门库可能没有定义类型声明，需要自己定义

总的来说，使用 TypeScript 是利大于弊的，TypeScript 已经成为了前端开发的主流。