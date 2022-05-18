# html

[直接看这里吧](https://juejin.cn/post/7095899257072254989)

# css

## 1. css选择器及优先级

1. id选择器（#id{}）
2. 类名选择器 （.class{}）
3. 属性名选择器 ( div[name='title'] )
4. 伪类选择器 ( div:after, div:before )
5. 子选择器（ div > span ）
6. 相邻元素选择器 ( div + div )
7. 标签选择器（div）
8. 后代选择器（div span）
9. 通配符选择器（*）

**优先级：**
!important > 内联式 > id选择器 > 类选择器 > 标签选择器 > 通配符 > 继承 > 浏览器默认



## 2. position 属性的值以及区别

1. 相对定位 - relative： 不脱离文档流， 在元素自身位置进行上下左右的移动。
2. 绝对定位 - absolute：脱离文档流，不占空间，相对于已定位的祖先元素的左上角进行上下左右的移动。如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块。
3. 固定定位 - fixed：脱离文档流，不占空间。相对于浏览器窗口固定位置，且不跟随滚动条移动。
4. 粘性定位 - sticky：先按照普通文档流，直到规定位置，然后粘在那里，不脱离文档流。当可以进入文档流时，再加入文档流。
5. 默认定位 - static：没有定位，元素出现在正常的流中。



## 3. css 盒模型

当对文档进行布局的时候，浏览器会根据基础框盒模型，将所有元素表示为一个个矩形的盒子，css决定这些盒子的大小，位置及属性。
每个盒子由四部分组成，内容区域-content、内边距 - padding、边框区域 - border、外边距 - margin。

1. 内容区域：容纳着元素的真实内容，例如文本，图像，它的尺寸为内容高度和内容宽度，如果box-sizing为默认的content-box。那么 css 指定的width和height为内容区域的宽度和高度，如果再设置边框和内边距，则会撑大这个元素。
2. 内边距区域：扩展自内容区域，负责延伸内容区域的背景，填充内容和边框的间距。
3. 边框区域： 扩展自内边距区域，容纳边框的区域。如果box-sizing设置为border-box，那么边框和内边距的值，是包含在设定的width和height中的，内容区域的宽度和高度就是这个设定的width和height减去边框和内边距的width和height。
4. 外边距区域： 利用空白区域扩展边框区域，用于和相邻元素分开。

## 4. 怎么触发BFC，有什么应用场景？

**BFC：** 块级格式化上下文

触发 BFC 条件：当元素满足以下任意一个条件的时候

* body 根元素
* 设置浮动，不包括 none
* 设置定位，absolute 或者 fixed
* 行内块显示模式：inline-block
* 设置 overflow，hidden、auto、scroll
* 表格单元格，table-cell
* flex

一个BFC区域，只包含其所有子元素，不包含子元素的子元素。

BFC有一个**特点**是：**每一个BFC区域都是相互独立，互不影响的。**

**利用 BFC 场景**

* 解决外边距塌陷问题

  当两个 div 的 margin 引起塌陷问题时，可以把他们分别放到两个 BFC 区域中。

  ```html
  <!-- 两个 margin 都是 100px 会引起塌陷 -->
  <div class="HT"></div>
  <div class="HT"></div>
  
  <!-- 利用 BFC 包裹起来 .HT_BFC => overflow:hidden -->
  <div class="HT_BFC">
    <div class="HT"></div>
  </div>
  <div class="HT_BFC">
    <div class="HT"></div>
  </div>
  ```

* 解决包含塌陷

  ```html
  <!-- son 设置 margig-top，会把 parent 一起带下去 -->
  <div class="parent">
    <div class="son"></div>
  </div>
  ```

* 清除浮动

  父元素设置 `overflow:hidden`

* 阻止标准流元素被浮动元素覆盖

  

## 5. flex 布局

flex 布局是弹性布局。任何元素都可以使用 flex 布局，包括行内元素。

父元素设为 flex 布局后，

* 其子元素的 float、clear、vertical-align 属性失效;
* 子元素默认沿主轴方向排列；

**父元素拥有以下几个属性**：

1. **flex-direction**：主轴的方向
   * row（默认值）： 主轴为水平方向，起点在左端
   * column：主轴为垂直方向，起点在上端
   * row-reverse：主轴为水平方向，起点在右端
   * column-reverse：column：主轴为垂直方向，起点在下端
2. **flex-wrap**：规定如果子元素在主轴方向放不下，该如何换行
   * nowrap：不换行，默认值
   * wrap：换行，第一行在上方
   * wrap-reverse：换行，第一行在下方
3. **flex-flow**：flex-direction 和 flex-wrap 的简写，默认值为 `row nowrap`
4. **justify-content**： 主轴上的对其方式，假设主轴方向从左到右：
   * flex-start：左对齐
   * flex-end：右对齐
   * center：居中
   * space-between：两端对齐
   * space-around：每个子元素两侧的间隔相等。（那么，项目与项目之间的间隔比项目与边框的间隔大一倍）
5. **align-items**：子元素沿交叉轴的对其方式
   * flex-start：沿交叉轴起点对其
   * flex-end：沿交叉轴终点对其
   * center：沿交叉轴中点对齐
   * baseline：沿子元素第一行文字的基线对齐
   * stretch（默认值）：如果子元素没设置高度或者设置为 auto，则占满容器的整个高度
6. **align-content**：子元素很多导致换行，有多根轴线时的对其方式。如果子元素只有一根轴线，则不起作用
   * flex-start：与交叉轴起点对齐
   * flex-end：与交叉轴终点对齐
   * center：与交叉轴中点对齐
   * space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
   * space-around：每根轴线两测间隔相等

**子元素拥有以下几个属性**：

1. **order**：子元素的排列顺序，数值越小，排列越靠前，默认为 0
2. **flex-grow**：子元素的放大比例，默认为 0。放大时所占用的空间是剩余空间
3. **flex-shrink**：子元素的缩小比例，默认为 1，负值无效
4. **flex-basis**：在分配多余空间时，子元素占父元素的主轴空间。默认值为 auto。可以设置为和宽度高度一样的值，则将占据固定的空间。
5. **flex**：flex-grow、flex-shrink、flex-basis 属性的简写。默认值为 `0 1 auto`

6. **align-self**：单个子元素在交叉轴上的对其方式，值和 align-items 的值一致，还有一个 auto，默认继承父元素的 align-items 的值，如果没有父元素，则为 stretch。

   

## 6. grid 布局

默认情况下，父元素都是块级元素（grid），但也可以设为行内元素（inline-grid）。

> 注意，设为网格布局以后，子元素（项目）的`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

**父元素属性**：

1. **grid-template-columns、grid-template-rows**：分别为：定义每一列的列宽、定义每一行的行高

   * repeat()：避免重复写同样的值，可以使用repeat 函数，定义重复的值

     接受两个参数，第一个参数是重复的次数（上例是3），第二个参数是所要重复的值。

     `grid-template-columns: repeat(3, 33.33%);`  // 生成 3 列，均为 33%宽度

     `grid-template-columns: repeat(2, 100px 20px 80px);`  可以生成 6 列，

   * **auto-fill**：自动填充，尽可能多的容纳单元格。

      `grid-template-columns: repeat(auto-fill, 100px);` // 每列宽度`100px`，然后自动填充，直到容器不能放置更多的列。

   * **fr关键字**：方便表示比例关系，如果两列的宽度分别为`1fr`和`2fr`，就表示后者是前者的两倍。

   * **minmax()**：函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

     `grid-template-columns: 1fr 1fr minmax(100px, 1fr);` => `minmax(100px, 1fr)`表示列宽不小于`100px`，不大于`1fr`。

   * **auto 关键字**：`auto`关键字表示由浏览器自己决定长度。

2. **grid-row-gap、grid-column-gap**：分别表示设置行与行的间隔（行间距）、设置列与列的间隔（列间距）

3. **grid-gap**：`grid-column-gap`和`grid-row-gap`的合并简写形式

   > 根据最新标准，上面三个属性名的`grid-`前缀已经删除，`grid-column-gap`和`grid-row-gap`写成`column-gap`和`row-gap`，`grid-gap`写成`gap`。

4. **grid-template-areas**

   ```css
   grid-template-areas: 'a b c'
                        'd e f'
                        'g h i';
   grid-template-areas: "header header header"
                        "main main sidebar"
                        "footer footer footer";
   ```

   > 注意，区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为`区域名-start`，终止网格线自动命名为`区域名-end`。
   >
   > 比如，区域名为`header`，则起始位置的水平网格线和垂直网格线叫做`header-start`，终止位置的水平网格线和垂直网格线叫做`header-end`。

5. **grid-auto-flow**：容器的子元素会按照顺序，自动放置在每一个网格。

   默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行。

   * `row` ："先行后列"
   * `column`，变成"先列后行"

   * `row dense`：表示"先行后列"，并且尽可能紧密填满，尽量不出现空格。
   * `column dense`：表示"先列后行"，并且尽量填满空格。

6. **justify-items、align-items**：分别表示：设置单元格内容的水平位置（左中右）、设置单元格内容的垂直位置（上中下）。

   - start：对齐单元格的起始边缘。
   - end：对齐单元格的结束边缘。
   - center：单元格内部居中。
   - stretch：拉伸，占满单元格的整个宽度（默认值）。

7. **place-items**：`justify-items` 和 `align-items`的简写：`place-items: <align-items> <justify-items>;`

8. **justify-content、align-content**：分别表示：整个内容区域在容器里面的水平位置（左中右）、整个内容区域的垂直位置（上中下）。

   - start - 对齐容器的起始边框。

   - end - 对齐容器的结束边框。

   - center - 容器内部居中。

   - stretch - 项目大小没有指定时，拉伸占据整个网格容器。

   - space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。

   - space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔。

   - space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。

9. **place-content**：`align-content`属性和`justify-content`属性的合并简写形式

   `place-content: <align-content> <justify-content>`

10. **grid-auto-columns、grid-auto-rows**：`grid-auto-columns`属性和`grid-auto-rows`属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与`grid-template-columns`和`grid-template-rows`完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

**子元素属性：**

1. grid-column-start、grid-column-end、grid-row-start、grid-row-end

   - `grid-column-start`属性：左边框所在的垂直网格线

   - `grid-column-end`属性：右边框所在的垂直网格线

   - `grid-row-start`属性：上边框所在的水平网格线

   - `grid-row-end`属性：下边框所在的水平网格线

     ```css
     .item-1 { /*1号项目的左边框是第二根垂直网格线，右边框是第四根垂直网格线。*/ 
       grid-column-start: 2;
       grid-column-end: 4;
     }
     ```

   这四个属性的值还可以使用`span`关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。

2. **grid-column、grid-row**：`grid-column`属性是`grid-column-start`和`grid-column-end`的合并简写形式，`grid-row`属性是`grid-row-start`属性和`grid-row-end`的合并简写形式。

   ```css
   .item-1 {
     grid-column: 1 / 3;
     grid-row: 1 / 2;
   }
   ```

3. **grid-area**：指定项目放在哪一个区域。

4. **justify-self、align-self**

   `justify-self`属性设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。

   `align-self`属性设置单元格内容的垂直位置（上中下），跟`align-items`属性的用法完全一致，也是只作用于单个项目。

   - start：对齐单元格的起始边缘。
   - end：对齐单元格的结束边缘。
   - center：单元格内部居中。
   - stretch：拉伸，占满单元格的整个宽度（默认值）。

这玩意属性太多了，还是单独整理成一个文档吧。

[CSS Grid 网格布局教程 - 阮一峰](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)



## 7. 元素水平垂直居中的方法有哪些？如果元素不定宽高呢？



# JavaScript

## 1. JavaScript 都有哪些数据类型

​	基本数据类型: `undefined` `null` `boolean` `string` `number` `symbol` `bigInt`

​	引用数据类型: `object`

​	区别: 基本数据类型存储在栈内存中，按值存取; 引用数据类型存储在堆内存中， 按引用存取。



## 2. 普通函数和箭头函数的区别

1. 原型，箭头函数没有原型，原型是 `undefined`

2. 箭头函数没有 `arguments`

3. `this` 箭头函数的 this 指向全局对象；普通函数中，this 的值取决于函数如何被调用的。

4. call、apply、bind 方法都改变不了箭头函数的指向， 第一个参数会被忽略

5. 箭头函数不能用作构造器，不能和 new 一起使用，因为：

   1. 箭头函数没有自己的 this
   2. 没有 prototype 属性，而在 new 执行时需要将构造函数的 prototype 重新赋值给 \__proto__

6. yield 关键字不能在箭头函数中使用（除非嵌套在允许使用的函数内），因此 ，箭头函数不能用作生成器

7. 返回值：箭头函数不加花括号（即箭头函数简写函数）时，默认返回函数体里面的内容；加花括号时，需要明确指定 return

   > 箭头函数简写时，直接返回一个对象字面量是行不通的，如果不想加花括号，又想使用简写函数返回，需要用括号把对象字面量包裹起来
   >
   > ```javascript
   > const func = ()=> { foo: 1 }  // error
   > const func = ()=> ({ foo: 1 }) // { foo: 1 }
   > ```



## 3. reduce 用法及实现

**用法：**

1. 数组累加
2. 利用 Math.max 获取数组最大值
3. 数组去重
4. 利用 `concat` 实现数组扁平化

[参考链接](https://juejin.cn/post/7011096419985522701)

**实现：**

```javascript
const arr = [1, 2, 3, 4, 5, 6]

const areduce = function (func, initValue) {
  if (typeof func !== 'function') throw Error('a error param in areduce')
  let iStartIndex = initValue !== undefined ? 0 : 1
  let iStartValue = initValue !== undefined ? initValue : this[0]
  let lastValue
  for (let i = iStartIndex; i < this.length; i++) {
    lastValue = func(lastValue ?? iStartValue, this[i], i, this)
  }
  return lastValue
}

const res = areduce.call(arr, (prev, next) => prev + next, 4)
console.log(res) // 25
```



## 4. jQuery和vue什么区别

vue 的核心思想是数据驱动，视图是由数据驱动产生。对视图的修改，不会直接操作 DOM， 而是通过数据修改。而 jQuery 是直接操作 DOM。vue 简化了操作 DOM 的代码量，只关系业务逻辑。

- vue 数据驱动
- jq 操作 dom 元素
- vue 简单或很少使用 dom
- vue 渲染优雅，代码易维护

那么为什么操作 DOM 会影响性能呢？



## 5. 为什么操作 DOM 会影响性能

在浏览器中，DOM 和 ES 是分离的，是两种东西。

 正因为二者相互独立，所以每次链接、每次访问DOM都会消耗性能 。

在此基础上，因为修改DOM会导致浏览器重新计算页面的几何变化、引发浏览器模板引擎的重排和重绘，进而更加消耗性能。

[参考链接](https://www.cnblogs.com/padding1015/p/11405788.html)



## 6. 事件循环

js 是一门单线程语言，可以使用事件循环实现单线程非阻塞

在 js 中，所有任务分为同步任务和异步任务，同步任务先进入主线程执行，执行完毕后再执行异步任务。但将任务划分为同步任务和异步任务不过准确，例如：

```javascript
console.log(1)

setTimeout(()=>{
    console.log(2)
}, 0)

new Promise((resolve, reject)=>{
    console.log('new Promise')
    resolve()
}).then(()=>{
    console.log('then')
})

console.log(3)
```

理想的结果是 ` 1 -> newPromise -> 3 -> 2 ->then`

但实际结果是 ` 1 -> newPromise -> 3 -> then -> 2`

原因是异步任务还可以细分为 宏任务 和 微任务

**宏任务：**

- script (可以理解为外层同步代码)
- setTimeout/setInterval
- UI rendering/UI事件
- postMessage、MessageChannel
- setImmediate、I/O（Node.js）

**微任务：一个需要异步执行的函数，执行时机是在主函数结束之后，当前宏任务结束之前， 也可以理解成当前宏任务结束之后，下一个宏任务开始之前**

- Promise.then
- MutaionObserver
- Object.observe（已废弃；Proxy 对象替代）
- process.nextTick（Node.js）

**运行机制**

宏任务队列：`MacroTask Queue`

微任务队列：`MicroTask Queue`

- 执行一个宏任务（script 同步代码也是一个宏任务），如果遇到微任务，将其添加到 `MicroTask Queue`中
- 当前宏任务执行完毕，查找微任务队列中是否含有待执行的微任务，如果存在微任务，需将当前微任务队列的所有微任务（包括当前宏任务执行过程中遇到的微任务）执行完毕才能进行下一个宏任务
- 当前宏任务和微任务执行完毕，开始检查渲染，然后GUI线程接管渲染，渲染页面
- 页面渲染完毕，取出下一个宏任务，继续执行。

> **注意：** 被读取到主线程栈中的代码一定是**已经执行完成**的异步代码。
>
> 如果一个宏任务是异步任务，只有当此异步任务执行完成后，才会将其回调函数推送到任务队列中，供主线程栈读取。
>
> ```javascript
> setTimeout(()=>{
>   console.log(2)
> }, 2000)
> 
> setTimeout(()=>{
>   console.log(1)
> }, 1000)
> ```
>
> 上例代码执行结果为 `1 -> 2`
>
> 因为 1 先执行完毕，推入任务队列中；2 在 1 之后执行完毕

[js中的宏任务与微任务](https://zhuanlan.zhihu.com/p/78113300)

[事件循环](https://interview.codespring.top/JavaScript/event_loop.html)

[EventLoop和任务队列(微任务和宏任务)](https://www.jianshu.com/p/3f675a55417b)



## 7. 显示转换、隐式转换



## 8. 防抖

* 立即执行版

```javascript
function debounce(fn, time){
  let timer = null
  return function (){ 
    timer && clearTimeout(timer)
    !timer && fn.call(this, ...arguments)
    timer = setTimeout(() => {
      timer = null
    }, time)
  }
}
```

* 非立即执行版

```javascript
function debounce(fn, time){
  let timer = null
  return function (){
    timer && clearTimeout(timer)
    timer = setTimeout(() => {
      fn.call(this, ...arguments)
    }, time)
  }
}
```

* 最终版

```javascript
function debounce(fn, time, immediate){
  let timer = null
  return function (){
    timer && clearTimeout(timer)
    immediate && !timer && fn.call(this, ...arguments)
    timer = setTimeout(() => {
      immediate ? timer = null : fn.call(this, ...arguments)
    }, time) 
  }
}
```



## 9. 节流

* 定时器版

```javascript
function throttle(fn, time) {
  let timer
  return function(){
    if(timer) return
    timer = setTimeout(() => {
      fn()
      timer = null
    }, time);
  }
}
```

* 时间戳版

```js
function throttle(fn, time){
  let prev = 0
  return function(){
    let now = Date.now()
    if ( now - prev > time) {
      fn()
      prev = now
    }      
  }
}
```



## 10. 深浅拷贝

* 浅拷贝
  1. 等号赋值
  2. `Object.assign()`
  3. array 的一些方法

```javascript
function cloneshallow(target){
  if(typeof target === 'object' && target !== null){
    let cloneTarget = Array.isArray(target) ? [] : {}
    for (const key in cloneTarget) {
      if (target.hasOwnProperty(key)) {
        cloneTarget[key] = target[key]
      }
    }
    return cloneTarget
  }else{
    return target
  }
}
```



* 深拷贝

```javascript
// 针对对象循环使用 WeakMap ， 因为 WeakMap 只接受object作为键名，其中的引用类型都是弱引用类型，且不计入垃圾回收机制
function clonedeep(target, hash = new WeakMap()){
  if( target && target.constructor === Date ) return new Date(target)
  if( target && target.constructor === RegExp ) return new RegExp(target)
  if(typeof target === 'object' && target !== null){
    if(hash.has(target)) return hash.get(target)
    let allDescp = Object.getOwnPropertyDescriptors(target)
    let cloneTarget = Array.isArray(target) ? 
      [] : Object.create(Object.getPrototypeOf(target),allDescp)
    hash.set(target,cloneTarget)
    for (const key of Reflect.ownKeys(target)) {
      // target[key] === target ?  // 对于对象循环，也可通过做判断来解决
      // cloneTarget[key] = cloneTarget :
      cloneTarget[key] = clonedeep(target[key],hash )
    } 
    return cloneTarget
  }else{
    return target
  }
}
```



## 11. 原型和原型链

**原型**： 每一个函数都会默认有一个 prototype 属性，指向原型对象，原型对象默认只会获得 constructor 属性，这个 constructor 指向的是这个构造函数。通过构造函数 new 出来的实例都会有一个 [[prototype]]，指向构造函数的原型对象。脚本没有访问 [[prototype]] 的方式，但浏览器，例如 chrome，safari，火狐，会在每个实例上暴露一个 \__proto__ ,用于指向原型，也就是构造函数的原型对象。

`isPrototypeOf`，判断是否是某个实例的原型

`Object.getPrototypeOf`， 获取某个实例的原型

`Object.setPrototypeOf`，为实例设置原型

**原型链：**

在读取实例上的属性时，首先会在实例本身上面查找，如果没有找到，就会去找该实例的 `__proto__` ，也就是构造函数的 prototype。

如果实例的 `__proto__` 上没有， 这个 `__proto__` 也是某个类型的实例，于是去找 `__proto__` 的 `__proto__`，直到 Object 的 prototype，也就是 null， 这就是原型链。



## 12. for...in 和 for...of

* **`for...in`**

  `for...in` 会遍历自身的可枚举属性，以及原型链中的可枚举属性。如果想只遍历自身属性，可以使用 `hasOwnProperty` 判断。

  `for...in` 一般适用于遍历对象，遍历数组时不一定按照次序遍历。

* **`for of`**

  `for...of`是 ES6 新引进的语法，用于遍历可迭代对象。适用于遍历数组。遍历对象报错。



## 13. 创建对象的各种方式

* **工厂模式**

  ```js
  function createPerson(name, age, job) {
    let o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
      console.log(this.name);
    };
  	return o; 
  }
  ```

* **构造函数模式**

  ```js
  function Person(name, age, job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function() {
      console.log(this.name);
  }; }
  let person1 = new Person("Nicholas", 29, "Software Engineer");
  ```

### 	new 操作符干了什么?

1. 在内存中创建一个新对象。

2. 这个新对象内部的[[Prototype]]特性被赋值为构造函数的 prototype 属性。

3. 构造函数内部的 this 被赋值为这个新对象（即 this 指向新对象）。

4. 执行构造函数内部的代码（给新对象添加属性）。

5. 如果构造函数返回非空对象，则返回该对象;否则，返回刚创建的新对象。

   构造函数的缺点是，定义的方法都会在每个实例上都定义一遍。各个实例虽然函数同名，但并不是同一个 Funtion 实例。解决方法是在内部定义方法时指向外部的同一个方法。

   ```js
   function newFunc(constructer){
     const instance = Object.create(null)
     const restArgs = Array.prototype.slice.call(arguments, 1)
     instance.__proto__ = constructer.prototype
     
     const result = constructer.apply(instance, restArgs)
     return typeof result === 'object' ? result : instance
   }
   ```

* **原型模式**






## 14. == 和 === 的区别

* **==** 

  == 在比较之前，会先进行类型转换，直到类型相同，在确定操作数是否相等。遵循以下规则：

  1、如果两个值类型相同，进行 === 比较。 

  2、如果两个值类型不同，他们可能相等。根据下面规则进行类型转换再比较： 

    a、如果一个是null、一个是undefined，那么[相等]。 

    b、如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。 

    c、如果任一值是 true，把它转换成 1 再比较；如果任一值是 false，把它转换成 0 再比较。 

    d、如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。js核心内置类，会尝试valueOf先于toString。

    e、任何其他组合，都不相等。

* === 

  === 只有在不转换类型的前提下相等才返回 true，类型必须相同，值也必须相同



## 15. 



## 16. js 中字符串常用方法

* `indexOf` ：查询字符串中是否含有某个内容 ， 返回字符串中的指定位置，未找到返回 -1
* `concat`： 链接字符串，返回连接后的字符串，不影响原来的字符串
* `split`：将字符串按某个字符拆分成一个数组，返回这个数组
* `toUpperCase`：转换成大写，返回它
* `toLowerCase`：转换成小写，返回它
* `trim`: 去掉前后空格



## 17. 数组常用方法

* `forEach` 遍历数组，传一个回调函数，第一个和第二个参数分别是成员和索引

* `map` 遍历数组，传一个回调函数，第一个和第二个参数分别是成员和索引，返回由回调函数的返回值组成的数组。

* `find` 遍历数组，查询某个元素，传一个回调函数，第一个和第二个参数分别是成员和索引，当回调函数返回值为 true 时，查询停止，并且返回这个当前满足条件的成员

* `findIndex` 用法同 find，返回 find 中这个满足条件的成员的索引

* `some` 遍历数组，回调函数用法一样，当回调函数返回第一个 true 时，停止遍历，并且返回 true

* `every` 遍历数组，回调函数用法一样，只有当回调函数都返回 true，every 函数才会返回 true ，否则返回 false

* `filter` 

* `includes` 传入两个参数，要搜索的值和开始搜索的位置的索引，查询数组中是否包含这个成员。includes 采用的是三等运算符（===），但和 === 的区别是对 NaN 的处理不同，=== 中 NaN === NaN 是 false， 但 includes 是可以搜索到的。

* `reduce` 接受两个参数：

  第一个参数是一个回调函数，这个回调函数有三个参数，prev、current、index ，表示前一个值、当前值、索引

  reduce 的第二个参数可选，是回调函数的前一个值

  如果传了第二个参数，那么将从数组的第一个成员开始循环，索引从 0 开始；如果不传，那么将从第二个成员开始循环，索引从 1 开始。

* `reduceRight` 和 reduce 用法一样，从后往前循环。
* `join` 接受一个参数，将数组以这个参数分隔连成一个字符串
* `slice` 截取数组，接受两个参数，startIndex、endIndex，从 startIndex 开始截取，到 endIndex 结束，包含 startIndex，不包含 endIndex，返回这一区间内的所有成员
* `splice` 删除或替换数组指定位置的成员，会修改原数组，接受 n 个参数，第一个参数是开始替换的位置索引，第二个参数是替换的个数，第三个到第 n 个参数是要填充的成员（可选），若没有要填充的成员，则表示删除，返回被替换掉的区间的所有成员组成的数组。
* `indexOf` 查询数组中是否包含指定的成员，返回索引。使用 === 比较。和 includes 不同的是，indexOf 比较时，NaN === NaN 为 false，includes 为 true
* `fill` 填充数组 (value [, start, [ end ] ])  
* `sort` **排序，看看资料吧**。
* `reverse` 数组反转，影响原数组
* `push` 向数组尾部增加成员
* `unshift` 将一个或多个元素添加到数组的**开头**，并返回该数组的**新长度(该**方法修改原有数组**)**。 
* `shift` 从数组中删除第一个成员，并返回该成员
* `pop`  从数组中删除最后一个元素，并返回该元素的值。此方法更改数组的长度。



## 18. JavaScript中执行上下文和执行栈是什么

全局执行上下文和函数执行上下文
1. 程序启动，执行全局代码，创建全局上下文，全局上下文被压入执行上下文栈。

2. 全局上下文初始化：VO（变量对象）、scope（作用域）、this

3. 全局上下文初始化的同时，函数被创建，则将全局上下文的作用域链保存到函数的`[[scope]]`属性中

4. 函数执行，创建函数执行上下文，函数执行上下文被压入执行上下文栈

5. 初始化函数执行上下文， 

  （1） 复制函数的 `[[scope]]`  属性创建作用域链 

  （2） 用`arguments`创建活动对象

  （3） 初始化活动对象，加入形参，函数声明，变量声明。

  （4） 将活动对象压入 函数的作用域链的顶端

6. 函数执行完毕，函数执行上下文从执行上下文栈中弹出

[[JavaScript深入之执行上下文]](https://github.com/mqyqingfeng/Blog/issues/8)



## 19. 谈谈你对作用域和作用域链的理解

1. **作用域**

   作用域是变量的适用范围，作用域决定了代码区块中变量的可见性。比方说函数作用域，函数内所有定义的变量都只能在函数内部使用，在函数外无法

   问。

   作用域主要有静态作用域（也就是词法作用域）和动态作用域

   js采用的是静态作用域，特点是作用域在函数定义的时候就已经确定了。动态作用域是在函数执行时才确定作用域。

2. **作用域链**

   当查找变量的时候，会先从当前执行上下文的变量对象中查找，如果没找到，就会去父级（词法层面的父级，并非谁调用的，谁就是父，而是看函数定义时

   的父级）执行下上文中去查找，一直到全局执行上下文中，直到找到为止。这样由多个执行上下文变量对象构成的链表，就叫做作用域链。

   函数有一个`[[scope]]`属性，里面放着父级的作用域链，函数执行时，初始化函数的执行上下文，创建活动对象，将活动对象添加到`[[scope]]`的最前面，那

   么，那么这个函数的作用域链就生成了。

[JavaScript深入之作用域链](https://github.com/mqyqingfeng/Blog/issues/6)



## 20. 变量对象

变量对象在执行上下文中，里面是变量和函数的声明。

不同的执行上下文它的变量也不同，有全局执行上下文中的变量对象和函数执行上下文中的变量。

全局执行上下文的变量对象在代码的任何地方都能访问到，因为按照作用域链会一直找到全局执行上下文。

函数执行上下文的变量对象是在进入函数执行上下文时生成的，此时函数还未执行，初始化活动对象（活动对象和变量对象是一个东西）包括形参，变量声明、

函数声明，变量声明和函数声明的值此时为`undefined`。当函数执行的时候，修改活动对象的值，为变量声明和函数声明赋值。



## 21. 说说你对闭包的理解，什么场景下使用闭包

闭包是指那些能够访问自由变量的函数。

自由变量是指在函数中使用，但既不是函数的参数也不是函数的局部变量。

这个是**理论角度**的闭包。

在**实践角度**，满足这两个条件才算闭包：

1. 即使创建它的上下文已被销毁，但这个函数依然存在。

2. 这个函数在代码中使用了自由变量。

闭包中之所以能够依然访问到自由变量，是因为`[[scope]]`属性。

在进入父函数的执行上下文中，会初始化作用域链，将父函数的作用域链（词法层面上的父级们的AO/VO）保存到子函数的`[[scope]]`属性中，即使父级函数执

行上下文销毁了，js依然可以让父级函数的执行上下文的活跃对象（AO）活在内存中。

就算把子函数返回出去，在子函数进入执行上下文时，会通过活跃对象（AO），和`[[scope]]`维护一个新的作用域链，将活跃对象（AO）放到最前面。所以子函

数依然可以从作用域链中找到那个自由变量，从而实现了闭包。 





## 20. 谈谈你对 this 的理解

* 对于普通函数，this 的指向由函数的执行环境决定。包括全局环境，对象环境，构造函数环境，事件对象。

  * 在全局环境下，this 指向的是全局对象
  * 在对象环境下，this 指向的是调用该函数的对象
  * 在构造函数环境下，this 指向的是实例对象
  * 在事件对象中，this 指向触发事件的 DOM 元素本身

  通过 call、apply、bind 可以改变函数 this 的指向

  ```js
  const oo1 = {
    a: 1,
    fn: function (){
      function fnn(){
        console.log(this);
      }
      fnn()
    }
  }
  
  oo1.fn() // window
  ```

  * **单独的函数调用指向 `window`** 这一点还没很理解

* 对于箭头函数的 this，是在箭头函数定义时就决定了的，指向的是定义时所在的作用域指向的对象

  其实箭头函数本身没有 this，如果使用 this 的话，this 指向的是上一级非箭头函数的 this

  另外，箭头函数不能通过 new 实例化对象。

  









## 21. js 中的事件模型

js中的事件模型有 DOM0级事件模型，DOM2级事件模型，IE事件模型

**DOM0级事件模型**：是一种原始的事件模型

​	**优点**：所有浏览器都兼容

​	**缺点**：相同事件的监听函数只能绑定一个，后绑定的会覆盖之前绑定的；没有事件冒泡、捕获

**DOM2级事件模型**：标准事件模型，事件发生包含三个过程，事件捕获阶段、事件目标阶段、事件冒泡阶段。

​	**事件捕获阶段：** 当某个元素出发事件，先从顶层 document 发出一个事件流，随着 DOM 树的节点向目标元素	流去，直到到达目标节点。

​	**事件目标阶段：** 到达事件目标，执行事件绑定的函数

​	**事件冒泡阶段：** 从目标函数往顶层元素传播

​	事件绑定方法：`addEventListener(事件类型，事件处理函数，true/false)`

​	移除监听：`removeEventListener(事件类型，事件处理函数，true / false)`

​	*第三个参数表示事件在何时被触发，在过程中的节点，如果设置了同类型事件并设置为 true，则表示该节点的事件在捕获阶段被触发； 为 false 则表示在冒泡阶段被触发，默认为 false*

​	**阻止捕获、冒泡：** 如果不需要触发捕获或冒泡机制的话，可以阻止

​	`e.preventDefault()` 表示阻止捕获，某个节点阻止捕获并不影响事件流往下传播

​	`e.stopPropagation()` 表示阻止冒泡，某个节点阻止冒泡后将不会在往上传播

**IE事件模型**

IE事件模型有两个过程，事件处理阶段和事件冒泡阶段，这两个阶段和DOM2级事件类似。



## 22. typeof 和 instanceof 区别

`typeof` 只能判断基本类型，不能区分引用类型，如object 、array、null、它们都返回 object

instanceof 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上，但string、number、boolean 基本数据类型不能判断



## 23. bind、call、apply 区别？

* `call`：用一个指定的 this 值和一个或多个参数来调用一个函数。
* `apply`：用一个指定的 this 值和一个参数组成的数组来调用一个函数。
* `bind` ：创建一个新的函数，在bind被调用时，这个新函数的 this 被 bind 的第一个参数指定，其余参数作为新函数的参数供调用时使用。



## 24. 手写 bind、call、apply

```js
Function.prototype.call2 = function (content){
  const ctx = content || window
  const args = []
  for (let index = 1; index < arguments.length; index++) {
    args.push('arguments[' + index + ']');
  }

  function aSymbol(ctx){
    const symb = Math.random() + new Date().getTime()
    if(ctx.hasOwnProperty(symb)){
      return aSymbol(ctx)
    }
    return symb
  }

  const symb = aSymbol(content)
  content[symb] = this
  const result = eval('content[symb](' + args + ')')
  delete content[symb]
  return result
}
```



```js
Function.prototype.apply2 = function (content, paramsArr){
  const ctx = content || window
  const args = []

  function aSymbol(ctx){
    const symb = Math.random() + new Date().getTime()
    if(ctx.hasOwnProperty(symb)){
      return aSymbol(ctx)
    }
    return symb
  }
  const symb = aSymbol(content)
  content[symb] = this

  let result
  if(!paramsArr){
    result = content[symb]()
  }else{
    for (let index = 0; index < paramsArr.length; index++) {
      args.push('paramsArr[' + index + ']');
    }
    result = eval('content[symb](' + args.join(',') + ')')
  }

  delete content[symb]
  return result
}
```



```js
Function.prototype.bind2 = function(ctx){
  if (typeof this !== "function") {
    throw new Error("Function.prototype.bind2 - what is trying to be bound is not callable");
  }
  const originFn = this
  const bindParams = Array.prototype.slice.call(arguments, 1);
  
  const cb = function(){
    const args = Array.prototype.slice.call(arguments);
    // 当 cb 作为构造函数时，this指向的应该是实例，而不是ctx
    originFn.apply( this instanceof cb ? this : ctx, bindParams.concat(args));
  }
  //  如果直接赋值，那么在外面修改 cb.prototype 也会影响到 this.prototype，可以通过中间函数中转一下
  // cb.prototype = this.prototype
  const middleFn = function(){}
  middleFn.prototype = this.prototype;
  cb.prototype = new middleFn()
  return cb
}
```



## 25. 谈谈单点登录











# ES6

## 1. var、let、const 区别

1. 变量提升

   var 存在变量提升，let、const 不存在变量提升

2. 暂时性死区

   块级作用域内，js 引擎会注意出现在块后面的 let 声明，在这个 let 声明之前，不能以任何方式来引用声明的变量，这种在语法上就叫做**暂时性死区**。

3. 重复声明

   var 可以重复声明变量，但 let 和 const 在块级作用域内只能声明一次，其中 const 在声明时必须赋值且不能修改，let 声明后可修改

   

## 2. 介绍下 Set 和 Map

**Set**

* set 是一种新的数据结构，类似于数组，set 的成员都是唯一的，没有重复的值。

* Set 函数接受一个数组或者具有iterable接口的其他数据结构作为参数，用来初始化

* set 实例可以通过 add() 方法增加成员

* Set 加入成员的时候，不会发生类型转换，Set 内部判断相等的方法类似于 精确相等运算符 === ，不过和 === 主要的区别是NaN， set 会认为 NaN 等于自身， === 认为 NaN 不等于自身

* **Set 实例的属性：**
  * `size`：set 实例的成员总数
  * `constructor`： 构造函数

* **Set 实例的方法**

  * `add()`：向 set 实例中增加成员， 返回 set实例本身。
  * `has()`：接受一个参数，判断该参数是否在 set 实例内，返回一个布尔值
  * `delete()`：删除某个成员，返回一个布尔值，表示是否删除成功
  * `clear()`： 清空所有成员，无返回值

* Set 遍历操作

  * `keys()`：  返回一个键名遍历器
  * `values()`：返回一个键值遍历器
  * `entries()`：返回一个键值对遍历器
  * `forEach()`：无返回值，类似于数组forEach ， 接受第二个参数，绑定内部处理函数的 this 。

  由于set 没有键名，只有键值，所以 keys 和 values 一样，keys 返回的键名就是键值，entries 返回的键名和键值也是一样的

  

**WeakSet**

* WeakSet 和 Set 类似，不过 WeakSet 的成员只能是对象，不能是其他类型的值。

* WeakSet 中的对象都是弱引用，**垃圾回收机制不考虑WeakSet 对该对象的引用**。

* WeakSet不可遍历。因为 WeakSet 内部有多少成员，取决于垃圾回收机制有没有执行，执行前后很可能不一样。

* WeakSet 没有`size`属性，没有办法遍历它的成员。
* WeakSet 方法
  * add()
  * delete()
  * has()

> **垃圾回收机制**根据对象的可达性来判断回收，如果对象还能被访问到，就不会释放这块内存。当这个对象被用完之后，有时会忘记取消引用，导致内存无法释放，进而可能会造成内存泄漏。而 WeakSet 中的对象不计入垃圾回收机制，所以就不存在这个问题。只要WeakSet 在外部引用的这个对象在外部消失，那么WeakSet 里面的引用就会自动消失。

**Map**

Map 和对象类似，都是键值对的集合，不过对象只能用字符串作为键，Map 可以用各种类型作为键。

* Map 可以接受一个用来表示键值对的数组初始化。任何具有 iterable 接口、且成员都是一个双元素的数组的数据结构，都可以作为构造函数参数。set 和 map也可用来生成新 Map

* Map 的属性

  * `size`： 返回 Map 结构的成员总数。

* Map 的方法

  * `set(key, value)`： 返回整个 Map 结构。如果 key 已经存在，那么键值会被更新；否则新生成该键。
  * `get(key)`： 返回`key` 对应的 `value`，如果没找到，返回 `undefined`
  * `has(key)`： 返回一个布尔，表示某个键是否存在 map 中
  * `delete(key)`： 返回一个布尔，表示是否删除成功。
  * `clear()`：清空 map，无返回值

* map 的遍历方法

  * `keys()`：返回键名的遍历器。
  * `values()`：返回键值的遍历器。
  * `entries()`：返回所有成员的遍历器。
  * `forEach()`：遍历 Map 的所有成员。

  需要特别注意的是，Map 的遍历顺序就是插入顺序。

  

**WeakMap**

* WeakMap 与 Map 类似，主要区别有两点：

  * WeakMap 的键只能是对象，null 除外。

  * WeakMap 的键名所指向的对象，不计入垃圾回收机制

> 它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。

* WeakMap 与 Map 在 API 上的区别主要是两个
  * 没有遍历操作（即没有`keys()`、`values()`和`entries()`方法），也没有`size`属性。因为没有办法列出所有键名，某个键名是否存在完全不可预测，跟垃圾回收机制是否运行相关。这一刻可以取到键名，下一刻垃圾回收机制突然运行了，这个键名就没了，为了防止出现不确定性，就统一规定不能取到键名。
  * 无法清空，即不支持`clear`方法。因此，`WeakMap`只有四个方法可用：`get()`、`set()`、`has()`、`delete()`。



## 3. promise

1. **三种状态：**

* **进行中（pending）**
* **已成功（fulfilled）**
* **已失败（rejected）**

2. **一旦状态改变，就不会再变， 状态改变只能有两种情况，从 pending -> fulfiled; pending -> rejected。只要这两种情况发生，状态就凝固了，不会再变了，这时就称为 resolved（已定型）。**

3. `promise.then` `resolved` 时的回调方法

4. `promise.catch` `rejected` 时的回调方法

5. `promise.finally` 不管状态最终如何，都会调用此回调，且此回调不接受任何参数

6. `promise.all` 将多个 promise 实例，包装成一个 promise 实例。该方法接受一个数组或者一个 具有iterator 接口的数据作为参数，该数据的成员都必须为 promise 实例，如果不是，会先调用 promise.resovle 方法将参数转成实例。

   ```javascript
   const p = Promise.all([p1, p2 ,p3])
   ```

   p 的状态取决于p1、p2、p3

   当 p1、p2、p3 的状态都为 fulfilled 时，p 的状态才会为 fulfilled，此时，p1、p2、p3 的返回值组成的数组，传给 p 的回调函数。

   当 p1、p2、p3 中有一个状态为 rejected 时，p 的状态就为 rejected，此时，第一个被 rejected 的返回值传递给 p 的回调函数

   > 注意，当第一个被 rejected 的 promise 实例自己定义了 catch 方法，并不会调用 promise.all 的 catch 方法。

```javascript
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]
```

p1 先 resolved ，p2 rejected，并且执行 catch 方法，并返回一个新的 promise 实例，此时，p2 指向的是这个 catch 返回的新的 promise 实例，catch 执行完后，p2 也会 resolved ，因此 `Promise.all()` 方法参数里面的两个实例都会 `resolved` ，因此会调用`then`方法指定的回调函数，而不会调用`catch`方法指定的回调函数。

7. promise.race  

   `race： 速度竞赛/跑赢` 

   参数和 promise.all 一样，只要有一个状态先改变，promise.race 的状态就会变成这个第一个改变的 promise 的状态。

8. promise.allSettled

9. promise.any 参数和 promise.all 一样，只要有一个promise实例状态变为 fulfilled ，就会将该实例返回值，传给 promise.any 的回调函数；只有当所有的 promsie 实例都为 rejected 时，promise.all 状态才会变成 rejected

10. promise.resolve 将一个对象转成 Promise 对象

    * **参数是 promise 实例**

      原封不动的返回该 promise 实例

    * **thenable： 参数是一个含有 then 方法的对象**

      将该对象转成 promise 对象，然后立即执行该对象的 then 方法

    * **参数是不具有`then`方法的对象，或根本就不是对象**

      将该参数转为 promise 对象，状态为 resolved

    * **不带有任何参数**

      直接返回一个resolved 状态的 promise 对象
      
      

## 4. class

* ### 类定义

  

* ### 类构造函数

* ### 静态方法

  * 静态方法的 this
  * 方法重名
  * 可以继承

* ### 实例属性的新写法

* ## 静态属性

* ### 私有方法和私有属性

  判断方法

* ### new.target

* ### 继承

  * super

    子类在构造函数中必须调用一个 super方法，否则会报错。调用 super 方法会生成一个继承父类的this 对象，没有这一步就无法继承父类。

    **ES5 的继承机制**是，先创建一个子类的实例对象，再将原型指向父类的prototype，即“实例在前，继承在后”。

    **ES6 的继承机制**是先将父类的属性和方法添加到一个空对象中，再将该对象作为子类的实例。

    * super 作为函数调用

      super 作为函数调用只能在子类的构造函数中使用，在其他地方使用会报错。

    * super 作为对象使用

      在**普通方法**中，super 指向父类的原型对象。无法获取父类实例属性和方法。调用父类方法时，方法内部的 this 指向子类的实例

      在**静态方法**中，super 指向父类。调用父类方法时，方法内部的 this 指向当前子类，而不是子类实例。

  * 都有哪些会被继承？

    除了私有属性和方法，其余的都会被继承。

  * `Object.getPrototypeOf()`方法可以用来从子类上获取父类。

  *<u>先去回顾一下原型和原型链，在继续学习 [类的 prototype 属性和__proto__属性](https://es6.ruanyifeng.com/#docs/class-extends#%E7%B1%BB%E7%9A%84-prototype-%E5%B1%9E%E6%80%A7%E5%92%8C__proto__%E5%B1%9E%E6%80%A7)</u>*	

  

## 5. Proxy

Proxy 代理的意思。

Proxy 接受两个参数，第一个参数是被代理的对象；第二个是在对该对象做各种操作时的一个配置对象。比如取值，赋值。

想要让配置对象生效，需要对生成的实例进行操作，不能对目标对象进行操作。

**Proxy 支持的拦截操作：**

> **target**: 目标对象
>
> **propKey**：属性名
>
> **receiver**： proxy 实例本身

* `get(target, propKey, receiver)`：`get`方法用于拦截某个属性的读取操作，可以接受三个参数，其中最后一个参数可选。
* `set(target, propKey, value, receiver)`：拦截对象属性的设置，返回一个布尔值
* `has(target, propKey)`：拦截`propKey in proxy`的操作，返回一个布尔值。

一般只用到 get 和 set ，其他操作见[这里](https://es6.ruanyifeng.com/#docs/proxy)

**this 问题**：

proxy 的目标对象中的 this 指向的是 proxy 实例，并非目标对象本身，如果想指向目标对象本身，需要在拦截配置参数中 `this` 绑定原始对象



## 6. Iterator

1. **迭代器解决了什么问题**

   js 原来的数据结构本来有数组和对象，到 ES6 又有了set 和 map ，那么就需要一个统一的接口机制，来处理不同的数据结构。迭代器就是这种机制，它是一个接口，为不同的数据结构提供统一的访问机制。任何数据结构只要遍历iterator接口，就可以完成遍历操作。

2. **Symbol.iterator 函数规则**

   symbol.iterator是一个函数，它返回一个对象，这个对象有 next 方法。

   next 方法返回一个对象，这个对象有 value 和done 属性，其中done 是个布尔，当遍历到最后一个元素时，done 为 true，代表结束。

3. **iterator 的遍历过程**

   1. 创建一个指针对象，指向当前数据结构的起始位置。
   2. 第一次调用 next 方法，指针对象指向数据结构的第一个成员
   3. 第二次调用 next 方法，指针对象指向数据结构的第二个成员
   4. 不断地调用 next 方法，直到指针对象指向数据结构的结束位置

4. **默认含有Symbol.iterator的数据结构都有哪些**

   * string
   * array
   * set
   * map
   * **函数的 arguments对象**
   * **nodelist 对象**

5. **什么情况下会调用Iterator接口**

   * 使用 for...of 遍历数据时
   * 解构赋值
   * 扩展运算符
   * yield




## 7. Generator

1. **什么是生成器函数**

   在 function 关键字和函数名之间有一个星号，在函数体内部有 yield 表达式，这样就是一个生成器函数。

   执行生成器函数返回一个遍历器对象，每执行一个遍历器对象的 next 方法会依次执行到每一个 yield 表达式

2. **yield表达式**

   generator 函数返回带有 next 方法的遍历器对象。

   next 方法执行时，遇到 yield 表达式，就会暂停后面的操作，并把 yield 后面紧跟着的表达式的值作为 value 返回。

   再次调用 next，一直执行，直到遇见下一个 yield 表达式。

   如果没有遇到新的 yield 表达式，直接执行到 return，将return 后面的表达式的值作为 value 返回，并且 done 为 true；如果没有return 语句，则返回的 value 为 undefined

3. **Generator 与 Iterator 的关系**

   任意对象上的 Symbol.iterator 函数就是一个遍历器生成函数，而 generator 就是一个遍历器生成函数。

   所以，可以直接将 generator 函数赋值给任意对象的 Symbol.iterator 属性。

4. **next方法的参数**

   yield 表达式本身没有返回值，或者说只返回 undefined，next 方法可以传入一个参数，代表上一个 yield 表达式的返回值。

   ```javascript
   function *gen(){
   	const y = 2 + (yield 3)
   	yield y * 10
   }
   const a = gen()
   a.next() // 3
   a.next() // NaN
   a.next(3) // 50
   ```

5. **Generator 与 for...of 的关系**

   for...of 可以自动循环 generator 返回的遍历器对象。不用依次调用 next 方法。

6. **Generator.prototype.throw()**

   

7. **Generator.prototype.return()**

   generator 生成的遍历器对象可以执行 return 方法，用于终止遍历，该方法接受一个参数用于指定返回的值。

8. **yield***

   在 generator 函数体内，调用另一个 generator 函数，需要在前者函数体内部，手动完成 generator 的遍历操作。ES6 提供 yield* 表达式，用于在 generator 函数体内执行另一个 generator 函数。

9. 

## 8. async



## 







# vue

## 1. 生命周期
​	vue2: `beforeCreate` -> `created` -> `beforeMount` -> `mounted` -> `beforeUpdate` -> `updated` -> `beforeDestroy` -> `destoryed`

​	vue3: `beforeCreate` -> `created` -> `beforeMount` -> `mounted` -> `beforeUpdate` -> `updated` -> `beforeUnmount` -> `unmounted`

​	其中，`beforeCreate` 时还没有 vue 实例。`created` 时 dom 未被渲染。`beforeMount` 时 dom 未被渲染， `mounted` 时 dom 已被渲染。

| 生命周期      | 描述                                                         |
| ------------- | :----------------------------------------------------------- |
| beforeCreate  | 组件实例被创建之初，组件的属性生效之前                       |
| created       | 组件实例已经完全创建，属性也绑定，但真实 dom 还没有生成，$el 还不可用 |
| beforeMount   | 在挂载开始之前被调用：相关的 render 函数首次被调用           |
| mounted       | el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子    |
| beforeUpdate  | 组件数据更新之前调用，发生在虚拟 DOM 打补丁之前              |
| update        | 组件数据更新之后                                             |
| activited     | keep-alive 专属，组件被激活时调用                            |
| deactivated   | keep-alive 专属，组件被销毁时调用                            |
| beforeDestory | 组件销毁前调用                                               |
| destoryed     | 组件销毁后调用                                               |

![组件生命周期图示](https://gitee.com/haibolian/screenshot/raw/master/images/20220505134324.png)

## 2. 父子生命周期顺序
​	父 `beforeCreate` -> 父 `created` -> 父 `beforeMount` -> 子 `beforeCreate` -> 子 `created` -> 子 `beforeMount` -> 子 `mounted` -> 父 `mounted`

​	父组件先执行到 beforemount ，**之后等待子组件挂在完毕**，父组件才会执行 `mounted`

​	更新也如此，父 `beforeUpdate` -> 子 `beforeUpdate` -> 子 `updated` -> 父 `updated`

[参考链接](https://juejin.cn/post/6844903918753808398#heading-8)

## 3. 父子组件传值
  1. `props`，父组件用来给子组件传值。通过 `.sync` 子组件可以向父组件更新 `prop` 的值
  2. `$emit`，子组件可通过 `$emit` 发射带参数的事件向父组件传值
  3. `provide、inject`， 父组件可以通过 `provide` 向后代组件传值，后代组件通过 `inject` 接收。不过此种方式并非响应式
  4. `bus` 传值，`new` 一个新的 `vue` 实例，通过 `$on` `$emit` 利用事件传值
  5. 通过 `parent`、`children`、`ref` 获取组件实例，进行修改或者取值
  5. 通过 $attrs/$listeners 用于隔代的组件通信。$attrs 通常配合 inheritAttrs 一起使用
  6. `vuex`

## 4. nextTick

1. nextTick是等待下一次 DOM 更新刷新的工具方法。
2. vue有个异步更新策路，意思是如果数据变化，vue不会立刻更新DOM，而是开启一个队列，把组件更新函数保存在队列中，在同一事件循环中发生的所有数据变更会异步的批量更新。这一策路导致我们对数据的修改不会立刻体现在DOM上，此时如果想要获取更新后的 DOM 状态，就需要使用 nextTick。
3. 在Vue内部，nextTick 之所以能够让我们看到DOM更新后的结果，是因为我们传入的callback会被添加到队列刷新函数的后面，这样等队列内部的更新函数都执行完毕，所有DOM操作也就结束了，callback自然能够获取到最新的DOM值。

## 5. vue的data为什么是函数返回而不是直接一个对象

当同一个组件被重复使用的时候，如果修改了组件 1 中的数据，如果是一个对象，那么则是一个引用类型，那么数据组件 2 中的数据也会同步修改。利用函数返回对象就可以避免使用同一个对象，保证两个组件中的数据互不影响。

## 6. v-if 和 v-show 的区别

v-if 会从 dom 中删除元素

v-show 只是利用 css 的 display 来显示或隐藏元素

## 7. computed 和 watch 的区别和运用的场景？

* **computed** 是计算属性，依赖于其他属性值，并且 computed 的值有缓存，只有它依赖的属性值发生变化，下一次获取 computed 时，computed 才会重新计算，值才发生变化，这也是 computed 和 method 的一个重要的区别。
* **watch** 监听一个属性，每当此属性发生变化时，都会执行 watch 的回调函数。

当需要依赖多个属性进行计算或者组装时，可以定义一个 computed，多处使用该computde，只会执行一次，只有当内部依赖的属性发生变化时才会重新计算。

当一个属性变化时，需要执行异步或者开销较大的操作时，应该使用 watch。



## 8. 讲讲 vue 响应式原理

1. 定义一个响应式方法，参数为 data，循环 data 中的数据，通过 `Object.defineProperty` 为 data 中每个属性添加 get 和 set 方法。

   *此处需要注意的是，如果 get 方法直接 return data[key]的话，会导致栈溢出，解决方法是使用闭包， 将值作为函数的参数传递进来，get 和 set 都对这个参数进行操作。*

2. 在 Vue 构造器中，执行响应式方法，通过 Vue 参数中的 el 创建一个编译类的实例

3. 在编译类中，通过 el 获取到 dom，编译模板时，创建 Watcher 实例，添加到 Dep 实例中，收集依赖。 

   *Watcher 实例存储了 vue 实例，key，以及更新方法，在该方法中可以获取到 node。*

   *Dep 实例定义 notice 方法，可以遍历收集到的依赖进行更新*

   *可通过指定 Dep.target 是否有值，然后访问一下该属性的方式在响应式方法中将 watcher 实例添加到 Dep 中。*

4. 依赖收集完毕，接下来的动作就是派发更新了，在响应式函数的 `Object.defineProperty` 的 set 方法中，设置完值之后，通过 Dep 实例的notice 方法，遍历 watcher ，watcher 执行 update 方法。视图更新。

> **`Object.defineProperty`** 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
>
> ```javascript
> Object.defineProperties(obj, props)
> ```
>
> **`Object.defineProperties`** 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。
>
> ```javascript
> Object.defineProperty(obj, prop, descriptor)
> ```

[手写响应式代码](https://github.com/haibolian/code-grocery/blob/main/vue%20%E5%93%8D%E5%BA%94%E5%BC%8F.js)



## 9. 讲讲虚拟 dom

虚拟 dom 就是用 js 模拟真实的 dom 结构

利用虚拟 dom，可以减少dom 的操作次数，大大的提高了性能。

**虚拟 DOM 的实现原理**主要包括以下 3 部分：

- 用 JavaScript 对象模拟真实 DOM 树，对真实 DOM 进行抽象；
- diff 算法 — 比较两棵虚拟 DOM 树的差异；
- pach 算法 — 将两个虚拟 DOM 对象的差异应用到真正的 DOM 树。

**虚拟 DOM 的优缺点**

**优点：**

- **保证性能下限：** 框架的虚拟 DOM 需要适配任何上层 API 可能产生的操作，它的一些 DOM 操作的实现必须是普适的，所以它的性能并不是最优的；但是比起粗暴的 DOM 操作性能要好很多，因此框架的虚拟 DOM 至少可以保证在你不需要手动优化的情况下，依然可以提供还不错的性能，即保证性能的下限；
- **无需手动操作 DOM：** 我们不再需要手动去操作 DOM，只需要写好 View-Model 的代码逻辑，框架会根据虚拟 DOM 和 数据双向绑定，帮我们以可预期的方式更新视图，极大提高我们的开发效率；
- **跨平台：** 虚拟 DOM 本质上是 JavaScript 对象,而 DOM 与平台强相关，相比之下虚拟 DOM 可以进行更方便地跨平台操作，例如服务器渲染、weex 开发等等。

**缺点:**

- **无法进行极致优化：** 虽然虚拟 DOM + 合理的优化，足以应对绝大部分应用的性能需求，但在一些性能要求极高的应用中虚拟 DOM 无法进行针对性的极致优化。

## 10. 讲讲 diff 算法

1. 当组件创建和更新的时候， vue 会执行内部的 update 函数，该函数使用 render 函数生成虚拟的 dom 树，找到差异点，最终更新到真实dom

2. 将新旧对比差异的过程叫 *diff ,*vue 在内部通过一个叫做 patch 的函数来完成该过程。

3. 在对比的过程，vue 采用深度优先，同级比较的方式进行比较，同级比较就是说它不会跨越结构进行比较，在判断两个节点是否相同的时候，是根据虚拟节点的 key 和 tag 来进行判断的。

4. 具体来说，首先对根节点进行对比，如果相同则将旧节点关联的真实dom的引用挂到新节点上，然后根据需要更新属性到真实dom，然后再对比其子节点数组；如果不相同，则按照新节点的信息递归创建所有真实dom，同时挂到对应虚拟节点上，然后移除掉旧的dom。

5. 在对比其子节点数组时，vue对每个子节点数组使用了两个指针，分别指向头尾，然后不断向中间靠拢来进行对比，这样做的目的是尽量复用真实dom，尽量少的销毁和创建真实dom。如果发现相同，则进入和根节点一样的对比流程，如果发现不同，则移动真实dom到合适的位置。

6. 这样一直递归的遍历下去，直到整棵树完成对比。

[参考内容（写的很清晰）](https://juejin.cn/post/6958292554047553549)

## 11. 为什么 vue 检测不到给数组项赋值

在 vue2 中，vue 响应式是通过 `Object.defineProperty` 实现，而数组不可能都事先定义好数据，所以 vue 无法监听到。

可以通过 Vue.set 或者 vm.$set 对数组进行赋值

另外，vue 对数组的 push、splice 等一些方法也做了响应式处理，执行这些数组的方法也可以让 vue 检测到

## 12. 父组件如何监听子组件的生命周期

**两种方法**

* 通过 emit 事件方式：在子组件的生命周期中向父组件发射事件

  ```vue
  // Parent.vue
  <Child @mounted="doSomething"/>
      
  // Child.vue
  mounted() {
    this.$emit("mounted");
  }
  ```

* 通过 hook 的方式

  ```vue
  //  Parent.vue
  <Child @hook:mounted="doSomething" ></Child>
  
  doSomething() {
     console.log('父组件监听到 mounted 钩子函数 ...');
  },
      
  //  Child.vue
  mounted(){
     console.log('子组件触发 mounted 钩子函数 ...');
  },    
      
  // 以上输出顺序为：
  // 子组件触发 mounted 钩子函数 ...
  // 父组件监听到 mounted 钩子函数 ...   
  ```

## 13. 谈谈你对 keep-alive 的了解

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，避免重新渲染，当再次加载组件时，会从缓存中获取该组件 ，其有以下特性：

- 一般结合路由和动态组件一起使用，用于缓存组件；
- 提供 include 和 exclude 属性，两者都支持字符串或正则表达式， include 表示只有名称匹配的组件会被缓存，exclude 表示任何名称匹配的组件都不会被缓存 ，其中 exclude 的优先级比 include 高；
- 对应两个钩子函数 activated 和 deactivated ，当组件被激活时，触发钩子函数 activated，当组件被移除时，触发钩子函数 deactivated。

> 如果多个组件内容比较简单，也可以使用 v-show 的方式替代 keep-alive，如果比较复杂，太影响首次加载的性能时，可以使用 keep-alive

## 14. v-model 的原理

v-model 的本质只不过是一个语法糖，v-model 在内部为不同的输入元素使用不同的属性向外抛出不同类型的事件

* text 和 textarea 元素使用 value属性 和 input事件
* checkbox 和 radio 使用 checked 属性和 change 事件
* select 字段将 value 作为 prop 并将 change 作为事件

在自定义组件中，默认使用 value属性 和 input事件

## 15. vue-router 中 hash 和 history 路由模式实现原理

**hash模式实现原理**

* location.hash 就是 url # 后面的内容，向服务器发送请求时，hash 部分不会被发送，hash 值的变化会在浏览器中记录，可以通过前进后退来进行 hash 值的改变。

* 可以通过 a 标签或 JavaScript 来对 hash 进行赋值。
* 通过 hashchange 来监听 hash 值的变化，从而对页面进行跳转。

**history 模式实现原理**

history 模式使用 HTML5 提供的history API 来实现 url 的变化。主要使用的 API 是 `history.pushState()` 和 `history.repalceState()` 。这两个 API 都可以在不刷新页面的情况下，实现 URL 的改变。`history.pushState ` 是新增一个记录，`history.replaceState` 是替换当前的历史记录。

history 模式的实现主要基于以下几个特性：

* `history.pushState` 和`history.replaceState` 两个 API 来操作实现URL 变化；
* 使用 `popstate` 事件监听 URL 变化，从而对页面进行渲染；
* 但是呢 `history.pushState` 和`history.replaceState`的调用并不会触发 `popstate` 事件。只有在做出浏览器动作时，才会触发。这时我们需要手动触发页面跳转（渲染）。比如在 js 中调用  `history.back` 。

## 16. MVVM

MVVM 分为 View 层、ViewModel 层、Model 层。

* **View 层：**视图层，用户看到的界面
* **Model 层：**数据模型层，也就是后端的业务逻辑、数据操作。对前端来说就是后端提供的接口 API，
* **ViewModel 层：**视图数据层，在这一层，前端对后端接口返回的数据进行二次封装，生成符合 View 层的数据。在 ViewModel 层所封装出来的数据包括状态和行为，状态也就是 View 层所需要显示的数据，行为也就是用户和 View 层交互时发生的事件，比如各种事件，点击时的行为。

MVVM 模式促进了前端页面和后端业务逻辑处理的分离，MVVM 的核心是 ViewModel ，前端开发只需要重点关注 ViewModel 层，后端开发也只需要关注业务逻辑。View 层展示的数据也不在是Model 层，而是 ViewModel 层对 Model 层处理后的数据，这样就完全解耦了 View 层和 Model 层。这也是前后端分离的重要一环。	

## 17. Vue 中的 key 有什么作用

key 是 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速。



## 18. Vue 性能优化方案

1. 区分 v-show、v-if
2. v-for 加 key
3. 根据具体情况合理使用 computed、watch
4. 大量不需要响应式的数据，不需要响应式的数据就不需要 Vue 对其进行数据劫持，可以使用 `Object.freeze` 方法冻结对象。减少初始化的时间。
5. 事件销毁，在 Vue 组件销毁时，vue 会自动清理它的实例连接，解绑这个组件的指令、事件监听器，但是这只限于组件的本身，在 mounted 时，使用 dom 定义的addEventListener，Vue 并不会自动销毁，这时，我们需要手动销毁这些事件监听，以免造成内存浪费和内存泄漏。
6. 图片资源的懒加载。
7. 使用精灵图。
8. 路由的懒加载。
9. 第三方插件的按需引入，如 Element 组件如果用的不多的情况下，可以按需引入。

## 19. proxy 和 Object.defineProperty 优劣对比

`Object.defineProperty` 的优势是兼容性好

`proxy` 的优势就比较多了：

1. proxy 可以直接监听对象而非属性
2. proxy 可以监听数组的变化
3. proxy 返回的是一个新的对象，我们可以操作这个新的对象从而达到目的。而 `Object.definePropery` 只能通过遍历对象的属性进行修改。

## 20. SPA首屏加载速度慢的怎么解决

1. Vue-Router路由懒加载（利用Webpack的代码切割）
2. 使用CDN加速，将通用的库从vendor进行抽离
3. Nginx的gzip压缩
4. Vue异步组件
5. 服务端渲染SSR
6. 如果使用了一些UI库，采用按需加载
7. 图片懒加载减少占用网络带宽
8. 页面使用骨架屏
9. 利用好script标签的async和defer这两个属性。功能独立且不要求马上执行的js文件，可以加入async属性。如果是优先级低且没有依赖的js，可以加入defer属性。

可利用`performance.timing`看各个步骤的耗时：白屏时间：`performance.timing.responseStart \- performance.timing.navigationStart`

## 21. 



# vuex

## 1. Vuex有哪些基本属性，为什么mutation不能做异步操作

* state：用来存放一些共享的数据
* mutations：存放修改数据的方法，同步
* actions：发送异步请求
* getters：类似于computed，基于 state 创建新的数据
* modules：模块化，可以创建命名空间，每个模块维护自己的状态。

因为开发过程中，我们常常会追踪状态的变化，如果在mutation中通过异步操作来更新state中的数据，虽然在页面的显示效果上没有什么影响，但是会导致开发工具不能追踪到状态的改变，调试起来会很困难。

# http

## 1. 跨域解决方案
  1. `webpack` 中的 `proxy` 代理

  2. `nginx` 反向代理

  3. `jsonp`：通过 `script` 标签发送网络请求，利用回调函数获取数据

       前端创建 script 标签，src 为请求地址，在请求参数中，指定回调函数名称。回调函数的参数用来接收数据。例如： `src="localhost:3000/users/getInfo?username=admin&callback=func"`

       服务端返回该回调函数名称 + 数据 组成的结果。例如：`res.end('func(' + data + ')');`

       **缺点：**只支持 get 请求。

       [参考链接](https://blog.csdn.net/badmoonc/article/details/82289252)

  4. `postMessage`： `html5` 引入了`message` 的 api ，方便、有效、安全。`postMessage` 方法允许来自不用源的脚本采用异步的方式进行通讯。可以实现跨文本档、多窗口、跨域消息传递。

       postMessage 接收两个参数：

       - `data` ：要传递的数据。
       - `origin`：目标窗口的源。

       [参考链接](https://www.cnblogs.com/dolphinX/p/3464056.html)

  5. `cors` 跨域资源共享。 `cors` 分为简单请求和非简单请求。
     **简单请求：**

     1. 请求方式为 HEAD、GET、POST 三者之一
     2. http 头部信息不超过以下几种字段
     * Accept
     * Accept-Language
     * Content-Type: 只限于 application/x-www-form-urlencode、multipart/form-data、text/plain 三种
     * Content-Language
     * Last-Event-ID

     **非简单请求：** 不满足以上两种条件，均为非简单请求
     **简单请求处理方式：** 对于简单请求，浏览器直接发出 CORS 请求，并自定在头部信息增加一个字段：Origin，用来说明本次请求来自哪个源（协议 + 域名 + 端口）；服务器判断该 Origin 决定是否同意本次请求，若同意本次请求，则在响应中添加三个与 CORS 相关的字段，均以 `Access-Control-` 开头：

       1. `Access-Control-Allow-Origin`：必选，本次请求的 Origin 的值 或者 *

       2. `Access-Control-Allow-Credentials`：可选，布尔值，表示是否允许发送 cookie。默认情况下，cookie 不包含在 CORS 中。

       3. `Access-Control-Expose-Headers`：可选。CORS 请求时，XMLHttpRequest 对象的 getResponseHeader 方法只能拿到 6 个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，必须在 `Access-Control-Expose-Headers` 中指定。getResponseHeader使用方法例如 getResponseHeader('FooBar')

       4. withCredentials： 如果要把 cookie 发送给服务器，一方面要经过服务器的同意，即服务器设置了 `Access-Control-Allow-Credentials = true`。另一方面，前端开发也需要在 ajax 打开 withCredentials 属性。

            > 需要注意的是，如果要发送 cookie `Access-Control-Allow-Origin` 不能设置为星号（*），必须指定明确的，与请求网页一致的域名。同时，coolie 也遵循同源政策，只有用服务器域名设置的 cookie 才会上传，其他域名不会上传。

     **非简单请求的处理方式：**

     1. 预检请求：在 CORS 发送正式请求之前，会先对服务器发送一个 method 为 OPTIONS 的预检请求，先询问服务器，该网页所属的域名是否在服务器允许的范围中，以及可以使用哪些 HTTP 动词和头信息字段。只有得到正确的答复，浏览器才会发送正式的请求。

        预检请求的头部信息包含两个特殊的字段：

        **`Access-Control-Request-Method `**：必选，用来列出浏览器的 CORS 请求会用到哪些 http 方法。

        **`Access-Control-Request-Headers`** ：可选，逗号分隔，指定浏览器的 CORS 请求会额外发送的头部信息。

     2. 预检请求回应：服务器收到预检请求后，根据请求头部信息做出相应处理。关键的是 `Aceess-Control-Allow-Origin` 表示允许请求的域名。服务器回应的其他字段如下：

        `Access-Control-Allow-Methods`

        `Access-Control-Allow-Headers`： 如果浏览器发送的请求包含`Access-Control-Request-Headers` ，则该项是必需的。

        `Access-Control-Allow-Credentials`

        `Access-Control-Max-Age`：表示此次预检请求的有效期，在此有效期间，不用发送另一条预检请求。

     [参考链接](https://www.ruanyifeng.com/blog/2016/04/cors.html)

## 2. 强缓存和协商缓存

* 强缓存

  浏览器第一次访问服务器，服务器返回一个 200 状态码，并且带有响应内容的返回，响应头中含有 cache-control，值为 max-age = ***，表示过期时间。还有 no-cache 也可以作为值。

  当浏览器第二次访问服务器时，如果访问时间在 max-age 之内，就会命中强缓存，返回 200 状态码，响应内容是缓存的内容。

* 协商缓存

  浏览器第一次发送请求，服务器返回 200 状态码和正确的资源，响应头中含有资源标识，就像是个资源版本，当浏览器再次访问服务器时，服务器会根据请求头中的资源标识和服务端的资源标识对比：

  如果相同则返回 304，浏览器从缓存中取得资源

  如果不同则同于一次完整的请求，返回 200 状态码，和正确资源。

  资源标识如下：

  * **Last-Modify/If-Modify-Since：**服务器返回的header中会加上Last-Modify，Last-modify是一个时间标识该资源的最后修改时间；当浏览器再次请求该资源时，request的请求头中会包含If-Modify-Since，该值为缓存之前返回的Last-Modify。
  * **Etag：**web服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识（生成规则由服务器决定）。
  * **If-None-Match：**当资源过期时（使用Cache-Control标识的max-age），发现资源具有Etage声明，则再次向web服务器请求时带上头If-None-Match （Etag的值）。web服务器收到请求后发现有头If-None-Match 则与被请求资源的相应校验串进行比对，决定是否命中协商缓存；

## 3. 三次握手

第一次握手：客户端向服务端发送 SYN(j) 建立连接请求，客户端进入 SYN_SEND 状态。*此时客户端有没有发送的能力尚未知晓*

第二次握手：服务端接收到客户端的请求，并向客户端发送 SYN(k)，和 ACK(j+1)，服务端进入 SYN_RECV 状态。*此时能确定客户端的发送能力和服务端的接收能力*

第三次握手：客户端接收来自服务端的发送的 SYN 和 ACK，并向服务端发送确认包 ACK（k + 1），这个包发送完毕，服务端和客户端进入ESTABLISHED 状态。*确定服务端的发送能力和客户端的接收能力*。完成三次握手，客户端可以向服务端发送资源请求。

[关于三次握手和四次挥手](https://baijiahao.baidu.com/s?id=1693383134922615393&wfr=spider&for=pc)

## 4. 四次挥手

第一次挥手：客户端向服务端发送 FIN 报文，请求连接终止(FIN = 1)，此时客户端状态为 FIN_WAIT_1

第二次挥手：服务端接收到 FIN 报文后，可能还有没发送完的数据，于是先向客户端发送一个 ACK 报文，表明已经收到客户端的报文了，此时服务端处于 CLOSE_WAIT 状态

第三次挥手：服务端没有要发送的数据了，可以关闭了，于是服务端向客户端发送 FIN 报文，此时服务端处于 LAST_ACK 状态

第四次挥手：客户端收到FIN之后，会向服务端发送 ACK 作为应答， 此时客户端处于 TIME_WAIT 状态

## 5. http 常见状态码

[http 常见状态码](../http/http 常见状态码)

## 6. HTTP 请求报文结构





# 浏览器

## 1. 前端性能优化方案

* css

  1. 使用精灵图，将多个图标或图片放到一个图片中，通过 `background` 来显示相应的图片
  1. 移除空的 css 规则
  1. 充分利用 css 的继承属性，减少代码量
  1. 提取公共样式，减少代码量

* js

  1. 节流、防抖
  1. [图片懒加载](https://www.jb51.net/article/220662.htm)
  1. 如果需要添加很多 dom，先把节点通过 createElement 创建出来最后一次性加入 dom
  1. 批量绑定事件时，使用事件委托，利用事件冒泡使父节点实现。
  1. 当使用对象的多个或重复的某个属性时，使用ES6 的解构赋值，减少对对象的访问，尤其是 vue2 中的 this

* 网络

  尽可能减少 http 请求次数

## 2. 浏览器内核

浏览器内核主要分成两部分，渲染引擎和 js 引擎。

**渲染引擎**负责获取页面的内容，计算网页的显示方式，输出给显示器。浏览器内核的不同对于网页的语法和解释会有所不同，所以渲染效果也可能不同。

**js 引擎**则解析和执行 js 来实现网页的动态效果。

**常见的浏览器内核有**

IE：trident

Chrome：以前是 webkit，现在是Blink

火狐：Gecko

safari：webkit

opera：从 webkit 到 blink 

## 3. 从输入网址回车到页面显示过程

1. 通过 DNS 解析域名的实际 IP

2. 检查浏览器是否有强缓存，有则取本地的 html

3. 没有的强缓存，则与 web 服务器建立 tcp 链接，服务器通过请求参数检查是否协商缓存。有的话返回状态码 304，没有的话返回请求资源

4. TCP 协议通过三次握手建立链接

   1. 客户端发送 syn(j) 报文段请求链接，确定服务端是否开始端口准备链接。客户端状态为 SYN_SEND
   2. 服务器如果开启端口并准备接受链接，则会向客户端返回 SYN(k) + ACK(j+1) 报文段给客户端，服务端状态为 SYN_RECV
   3. 客户端收到 SYN + ACK 包，向服务端发送确认包 ACK(k+1)，这个包发送完毕，客户端和服务端进入ESTABLISHED状态，完成三次握手，客户端和服务端开始传输数据。

5. 建立链接后，浏览器请求页面资源

6. 服务器发送资源

7. 浏览器获取到资源后，解析HTML，如果解析到`style`标签则开始解析css，如果解析到link标签则先异步下载，完成后解析css。

   如果遇到`script`标签，判断是行内写法则直接解析执行，如果是src引入则`同步下载`脚本文件，下载完成`立即执行`，注意这里下载过程是`阻塞`的，其他流程都会等下载完成后执行。

8. 然后开始渲染页面，解析 HTML, 构建 DOM 树，然后解析 css，生成 css 规则树

   合并 DOM 树和 css 规则树，生成渲染树render

   浏览器开始布局 render树，根据各节点的大小，位置进行计算，得到每个节点的几何信息

   最后浏览器将各个节点的信息发送给GPU，GPU 绘制页面展示到显示器上。

## 4. cookies、sessionStorage、localStorage 和 indexDB 的区别

cookie是以文本的方式保存在客户端，每次请求时都带上它。 单个cookie保存的数据不能超过4kb。

 默认保存在内存中，随浏览器关闭失效（如果设置过期时间，在到过期时间后失效）

| 分类           | 生命周期                                                     | 存储容量                                   | 存储位置                                   |
| :------------- | ------------------------------------------------------------ | ------------------------------------------ | ------------------------------------------ |
| cookie         | 默认保存在内存中，随浏览器关闭失效（如果设置过期时间，在到过期时间后失效） | 4KB                                        | 保存在客户端，每次请求时都会带上           |
| localStorage   | 理论上永久有效的，除非主动清除。                             | 4.98MB（不同浏览器情况不同，safari 2.49M） | 保存在客户端，不与服务端交互。节省网络流量 |
| sessionStorage | 仅在当前网页会话下有效，关闭页面或浏览器后会被清除。         | 4.98MB（部分浏览器没有限制）               | 同上                                       |

**indexDB  和 webSQL** 是前端的一种数据库，没用过



## 5. 浏览器的渲染引擎工作原理

在浏览器下载完html、css、js图片后，就会开始渲染页面

1. 首先，解析HTML，构建DOM树
2. 然后解析CSS，生成CSS规则树
3. 然后合并DOM树和CSS规则树，生成一个渲染树，也就是render树
4. 开始布局render树，根据render树来对各个元素的大小，位置进行计算，得到每个节点的几何信息。它是根据浏览器视口的大小来计算元素的位置和大小。重排会走到这一步。
5. 开始绘制 render 树，绘制页面的像素信息，根据渲染树节点的几何信息，得到每个节点的像素数，重绘会走到这一步。
6. 最后浏览器将各个层次的节点信息发送给GPU，GPU绘制展示到页面上。

[参考资料1](https://juejin.cn/post/7075515261121626119)

[参考资料2](https://www.cnblogs.com/padding1015/p/11405788.html)



## 6. 重排和重绘

**重排**

当DOM变化影响了元素的几何属性，比如宽高，浏览器会重新计算元素的几何属性，同样的，和它相邻的元素的几何属性和位置也会受到影响，浏览器会对这些受影响的元素重新构造render树并布局render树。

**重绘**

重排之后，浏览器会重新绘制受影响的部分到屏幕中，会重新根据元素的几何信息计算像素数。这个过程就是重绘。



## 7. 哪些情况会导致重绘和重排，如何减少

**以下几个动作可能会导致性能问题**：

- 改变 `window` 大小
- 改变字体
- 添加或删除样式
- 文字改变
- 定位或者浮动
- 盒模型

**如何减少重绘和重排**：

1. 使用 `transform` 替代 `top`
2. 使用 `visibility` 替换`display: none` ，因为前者只会引起重绘，后者会引发回流（改变了布局）
3. 不要使用 `table` 布局，可能很小的一个小改动会造成整个 `table` 的重新布局
4. 动画实现的速度的选择，动画速度越快，回流次数越多
5. 将频繁重绘或者回流的节点设置为图层，图层能够阻止该节点的渲染行为影响别的节点。比如对于 `video` 标签来说，浏览器会自动将该节点变为图层。
6. 避免使用`css`表达式(`expression`)，因为每次调用都会重新计算值（包括加载页面）
7. 批量修改元素样式：`elem.className` 和 `elem.style.cssText` 代替 `elem.style.xxx`
8. 需要要对元素进行复杂的操作时，可以先隐藏(`display:"none"`)，操作完成后再显示
9. 需要创建多个`DOM`节点时，使用`DocumentFragment`创建完后一次性的加入`document`

[参考文章](https://mp.weixin.qq.com/s/gJQ81EVxjzm10yX_fxAp8w)

# webpack

## 1. 说说你对webpack的理解？解决了什么问题

webpack是js的静态模块打包工具

**解决了什么问题**

1. webpack可以实现模块打包，将各个模块之间的依赖关系构建依赖图，将各个模块打包成一个或多个js文件，实现前端模块化。
2. webpack可以解决浏览器兼容性问题，通过引入实现浏览器兼容的loader，通过这些loader实现浏览器兼容，还可以根据这些loader对一些开发中使用的文件类型进行编译转换，如less，sass，vue，jsx文件。



## 2. webpack的构建流程

1. 读取webpack配置文件参数
2. 根据配置参数创建compiler对象，开始解析项目
3. 从入口文件开始解析，调用所有配置的loader对模块进行翻译，找到其导入的依赖模块，递归遍历并通过loader处理。
4. 形成依赖关系树，根据入口和模块之前的关系，组装成一个个包含多个模块的Chunk，再把每个Chunk转换成一个单独的文件加入到输出列表。
5. 根据配置参数中的输出路径，把文件内容写入到文件系统。

## 3. webpack中常见的Loader？解决了什么问题？

1. sass-loader、less-loader：把scss和less代码转换成css
2. css-loader：负责解析css代码，支持模块化，css文件的导入的特性，并不会将解析后的css插入到页面中。
3. style-loader：把css-loader生成的内容用style标签挂载到页面的head中。
4. postcss-loader：处理css兼容，需要配合autoprefixer 和 postcss-preset-env 插件一起使用。 autoprefixer 和 postcss-preset-env 根据 browsersliset 返回的浏览器版本进行前缀添加。其实postcss-preset-env 中已经包含了 autoprefixer，所以直接用postcss-preset-env插件就行了。
5. babel-loader：把ES6语法转换成ES5
6. image-loader：加载并压缩图片文件
7.  file-loader：把识别出的资源模块，移动到指定的输出⽬目录，并且返回这个资源在输出目录的地址(字符串)
8.  raw-loader：在 webpack中通过 import方式导入文件内容，该loader并不是内置的，所以首先要安装
9. url-loader：可以处理理 file-loader 所有的事情，但是遇到图片格式的模块，可以选择性的把图片转成 base64 格式的字符串，并打包到 js 中，对小体积的图片比较合适，大图片不合适。



## 4. 说说webpack中常见的Plugin？解决了什么问题？

1. HtmlWebpackPlugin：自动生成一个 html 文件，并把打包后的 js 文件添加到里面。可以指定 html 模板。
2. clean-webpack-plugin：清理打包目录
3. DefinePlugin：在编译时创建配置的全局对象，是一个webpack内置的插件，不需要安装
4. copy-webpack-plugin：复制文件或目录到执行区域，如vue的打包过程中，如果我们将一些文件放到public的目录下，那么这个目录会被复制到dist文件夹中



## 5. 说说Loader和Plugin的区别

**Loader** 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。
因为 Webpack 只认识 js，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。

**Plugin** 就是插件，可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

Loader 在 module.rules 中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 test(类型文件)、loader、options (参数)等属性。

Plugin 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入。



## 6. 说说webpack的热更新是如何做到的？原理是什么

热更新就是在启动服务后，修改代码后，浏览器只针对修改的模块进行更新，并不影响其他模块。

当我们在编写还没有经webpack打包的源代码后，webpack Compile 将源代码和 HMR runtime 一起编译成bundle文件传给Bundle Server服务器，Bundle Server服务器就是一个静态资源服务器，HMR runtime 是一个socket服务器，它会被注入到浏览器，通过和HMR Server建立长连接，HMR server用来将更新的文件输出给HMR runtime，更新文件的变化。

当某一个文件发生变化时，webpack监听到文件变化对它重新打包，编译生成唯一的hash值，这个hash值用来作为下一次热更新的标识。

根据变化的内容，生成两个补丁文件，manifest （包含了hash和chunkId，用来说明变化的内容）和 chunk.js模块

服务器向浏览器推送消息，消息包含文件改动后的值和hash值和改动后生成的hash值。

浏览器拿到推送的消息之后，通过 HMR runtime机制，加载这两个文件，针对修改的文件进行更新。

[参考文档](https://vue3js.cn/interview/webpack/HMR.html#%E4%BA%8C%E3%80%81%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)

## 7. webpack proxy工作原理？为什么能解决跨域

浏览器和服务器之间可能会存在跨域，但服务器之间不存在跨域

因为webpack-dev-server 就是启动了一个本地服务器，所以浏览器发起的请求会让本地的服务器去代理发送，这样就不会产生跨域了。

然后通过proxy的配置，指定要代理请求的服务器和请求路径。

## 8. 如何借助webpack来优化前端性能？



## 9. 如何提高webpack的构建速度





# 阿里成本管理系统

## 1. 如果实现权限分配的

角色路由权限是放在了后端。

由前端定义好所有的路由，在路由配置页面配置好指定角色具有的路由。

登录后，前端通过接口拿到该用户的路由表，然后根据前端本地的路由比较，根据接口的路由表，对两个路由表的字段根据情况进行取舍，生成一份新的该用户的路由表，添加到 router 上面 并且存到stroe里面。

接口的路由表还可以配置一些按钮的权限，在路由表中的每个页面增加按钮编码组成的数组，前端把页面和按钮组组成一个映射放到store里，在页面里就可以根据该页面的按钮数组来判断是否渲染按钮。

## 2. 大数据表格的实现

1. 根据给定表格的高度，和表格每一行的高度，计算出表格能够看到的部分一共有多少行，表格 body 渲染时，就渲染这些数据。
2. 外层需要一个 div ，利用 `overflow: auto` 来显示出滚动条，这时就需要撑高这个表格，可是表格渲染的数据只有可视部分的行数。
3. 利用 `padding-top` 和 `padding-bottom` 来撑高这个表格，这时就需要计算这两个 padding 。需要用到两个索引，一个是 startIndex 和 endIndex， 分别表示可视部分最上面和最下面的所在行数的索引 ，比如可视部分是 10 行，那么startIndex 和 endIndex 分别是 0 和 9，那么利用这两个 index 和 总数据条数还有每一行的高度，就可以把这两个 padding 计算出来。表格的滚动条就出来了。
4. 添加滚动事件，根据事件的 target.scrollTop 属性重新计算startIndex 和 endIndex ，然后根据这个两个 index 截取数据给表格去渲染。

**使用 padding 导致重排问题：**

频繁更改 padding 会导致重排，影响性能，可以利用 translateY 来使表格进位位移：

1. 思路：表格 translateY，那么就不能利用表格来撑开高度了，可以使用一个空的 div，让这个空的 div 来撑开高度，进而让上一层的 div 显示滚动条
2. 空的 div 占据了整个高度，那么表格需要绝对定位来脱离文档流，给上一层 div 加个相对定位，这就在滚动时可以利用 translateY 来对表格进行位移了。

## 3. 手写金额格式化函数



## 4. 加减乘除的实现

核心思路是把 避免使用小数计算，两个要计算的数转换成整数 来进行计算。

将数组转成字符串，利用字符串的split方法计算小数位长度，利用replace将小数点去掉。

这时就可以用没有小数点的两个数进行计算，然后根据加减乘除的情况去乘以10的多少次方

## 5. 



