# 1. 基础



## 1. 响应式基础

1. 默认的，Vue 创建的响应式对象都是深层响应式的。也可以使用 shallowReactive 创建一个浅层响应式。

2. 响应式对象内的嵌套对象依然是代理

3. ```js
   const proxy = reactive({})
   
   const raw = {}
   proxy.nested = raw
   
   console.log(proxy.nested === raw) // false
   ```

4. **reactive 的局限性**

* 仅对对象类型有效（Object、Array、Set、Map、），而对stirng、number 等这样的原始类型无效

* 不能随意的替换要响应式的对象。因为 Vue 的响应式系统是通过 property 访问进行追踪的，因此我们必须始终保持对该响应式对象的相同引用。

  ```js
  let state = reactive({ count: 1 })
  
  // 这行不通
  state = reactive({ count: 2 })
  ```

5. **ref**

   ref 可以创建任意类型的响应式，ref 将参数包装成一个 value 为参数的 ref 对象

   ```js
   const count = ref(0)
   
   console.log(count) // { value: 0 }
   console.log(count.value) // 0
   
   count.value++
   console.log(count.value) // 1
   ```

   在模板渲染中，

   当 ref 作为文本差值计算的最终值的时候，才会自动解包，

   当 ref 作为顶层property时才会自动 "解包"，像下面这样

   ```js
   const obj = { foo: ref(0) }
   
   // template
   <p>{{ obj.foo }}</p> // 0
   <p>{{ obj.foo + 1 }}</p> // [object Object]1
   
   // 可以通过结构将 foo 置为顶层 property来解决
   // script
   const { foo } = obj
   // template
   {{ foo + 1 }} // 1
   ```

   当一个 `ref` 作为一个响应式对象(reactive)的 property 被访问或更改时，它会自动解包，因此会表现得和一般的 property 一样：

   ```js
   const count = ref(0)
   const state = reactive({
     count
   })
   
   console.log(state.count) // 0
   
   state.count = 1
   console.log(count.value) // 1
   ```



## 2. 计算属性

**computed 返回一个 计算属性 ref**

计算属性会自动追踪响应式依赖，只有当响应式依赖变化是，才会重新计算。

下面这个计算属性永远不会更新，因为 `Date.now()` 不是一个响应式依赖

```js
const now = computed(() => Date.now())
```

**计算属性是可修改的**

setter 和 getter

```js
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  // getter
  get() {
    return firstName.value + ' ' + lastName.value
  },
  // setter
  set(newValue) {
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})

fullName.value = 'John Doe'
</script>
```



> **计算函数不应该有副作用**
>
> 计算属性的计算函数应只做计算而没有任何其他的副作用，这一点非常重要，请务必牢记。举个例子，不要在计算函数中做异步请求或者更改 DOM！一个计算属性的声明中描述的是如何根据其他值派生一个值。因此计算函数的职责应该仅为计算和返回该值。在之后的指引中我们会讨论如何使用[监听器](https://staging-cn.vuejs.org/guide/essentials/watchers.html)根据其他响应式状态的变更来创建副作用。
>
> **避免直接修改计算属性值**
>
> 从计算属性返回的值是派生状态。可以把它看作是一个“临时快照”，每当源状态发生变化时，就会创建一个新的快照。更改快照是没有意义的，因此计算属性的返回值应该被视为只读的，并且永远不应该被更改——应该更新它所依赖的源状态以触发新的计算。



## 3. Class 与 Style 绑定

```
<div :class="[{ active: isActive }, errorClass]"></div>
```

## 4. 条件渲染

`v-show` 不支持在 `<template>` 元素上使用，也没有 `v-else` 来配合。

当 `v-if` 和 `v-for` 同时存在于一个元素上的时候，`v-if` 会首先被执行。请查看[列表渲染指南](https://staging-cn.vuejs.org/guide/essentials/list.html#v-for-with-v-if)获取更多细节。

## 5. 列表渲染

* 你也可以在定义 `v-for` 的变量别名时使用解构，和解构函数参数类似：

  ```vue
  <li v-for="{ message } in items">
    {{ message }}
  </li>
  
  <!-- 有 index 索引时 -->
  <li v-for="({ message }, index) in items">
    {{ message }} {{ index }}
  </li>
  ```

* 你也可以使用 `of` 作为分隔符来替代 `in`，这也和 JavaScript 的迭代器语法非常相似：

  ```vue
  <div v-for="item of items"></div>
  ```

* 你也可以使用 `v-for` 来遍历一个对象的所有属性。

  你也可以提供第二个参数表示属性名 (例如 key)：

  第三个参数表示位置索引：

  ```vue
  <ul>
    <li v-for="(value, key, index) in myObject">
      {{ index }}. {{ key }}: {{ value }}
    </li>
  </ul>
  <script>
    const myObject = reactive({
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    })
  </script>
  ```

  > 当遍历一个对象时，顺序是依据 `Object.keys()` 的枚举顺序，由于不同的 JavaScript 引擎可能会有不同的实现，所以顺序可能会不一致。

* **`v-for`** 与 **`v-if`** 

  当它们同时存在于一个节点上时，`v-if` 比 `v-for` 的优先级更高。这意味着 `v-if` 的条件将无法访问到 `v-for` 作用域内定义的变量别名：

* `key` 绑定的值期望是一个基础类型的值，例如字符串或 number 类型。不要用对象作为 `v-for` 的 key。要获取 `key` attribute 的更多用途细节，请看 [`key` API 文档](https://staging-cn.vuejs.org/api/built-in-special-attributes.html#key)。

## 6. 事件处理

有时我们需要在内联事件处理器中访问原生 DOM 事件。你可以向该处理器方法传入一个特殊的 `$event` 变量，或者使用内联箭头函数：

```vue
<!-- 使用特殊的 $event 变量 -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

<!-- 使用内联箭头函数 -->
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>
function warn(message, event) {
  // 这里可以访问原生事件
  if (event) {
    event.preventDefault()
  }
  alert(message)
}
```

**事件修饰符**

```vue
<!-- 单击事件将停止传递 -->
<a @click.stop="doThis"></a>

<!-- 提交事件将不再重新加载页面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰语可以使用链式书写 -->
<a @click.stop.prevent="doThat"></a>

<!-- 也可以只有修饰符 -->
<form @submit.prevent></form>

<!-- 仅当 event.target 是元素本身时才会触发事件处理器 -->
<!-- 例如：事件处理器不来自子元素 -->
<div @click.self="doThat">...</div>
```

```vue
<!-- 添加事件监听器时，使用 `capture` 捕获模式 -->
<!-- 例如：指向内部元素的事件，在被内部元素处理前，先被外部处理 -->
<div @click.capture="doThis">...</div>

<!-- 点击事件最多被触发一次 -->
<a @click.once="doThis"></a>

<!-- 滚动事件的默认行为 (scrolling) 将立即发生而非等待 `onScroll` 完成 -->
<!-- 以防其中包含 `event.preventDefault()` -->
<div @scroll.passive="onScroll">...</div>
```

`.passive` 修饰符一般用于触摸事件的监听器，可以用来[改善移动端设备的滚屏性能](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#使用_passive_改善的滚屏性能)。

TIP

> 请勿同时使用 `.passive` 和 `.prevent`，因为 `.prevent` 会被忽略并且你的浏览器可能会抛出警告。请记住，`.passive` 是向浏览器表明你*不想*阻止事件的默认行为。并且如果你这样做，可能在浏览器中收到一个警告。



* `.exact` 修饰符允许控制触发一个事件所需的确定组合的系统按键修饰符。

```vue
<!-- 当按下 Ctrl 时，即使同时按下 Alt 或 Shift 也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 仅当按下 Ctrl 且未按任何其他键时才会触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 仅当没有按下任何系统按键时触发 -->
<button @click.exact="onClick">A</button>
```



## 7. 表单输入绑定

- 文本类型的 `<input>` 和 `<textarea>` 元素会使用到 `value` 属性和 `input` 事件；
- `<input type="checkbox">` 和 `<input type="radio">` 使用 `checked` 属性和 `change` 事件；
- `<select>` 使用的 `value` 作为 prop，`change` 作为事件：

> `v-model` 会忽略任何表单元素上初始的 `value`，`checked` 或 `selected` attributes。它将始终将当前绑定的 JavaScript 状态视为数据的正确来源。你应该在 JavaScript 中声明该初始值，使用响应式系统的 API。

> 对于需要使用 [IME](https://en.wikipedia.org/wiki/Input_method) 的语言 (中文，日文和韩文等)，你会发现 `v-model` 不会在 IME 输入的组合状态时触发更新。如果你的确想在此时也触发更新，请使用 `input` 事件监听器和 `value` 绑定值而不要使用 `v-model`。

**修饰符**

`.lazy`

默认情况下，`v-model` 会在每次 `input` 事件后更新数据 ([IME composition 阶段的状态](https://staging-cn.vuejs.org/guide/essentials/forms.html#vmodel-ime-tip)例外)。你可以添加 `lazy` 修饰符来改为在每次 `change` 事件后更新数据：

```
<!-- 在 "change" 事件后同步更新而不是 "input" -->
<input v-model.lazy="msg" />
```

`.number`

如果你想让用户输入自动转换为数字，你可以在 `v-model` 后添加 `.number` 修饰符来管理输入：

如果该值无法被 `parseFloat()` 处理，那么将返回原始值。

`number` 修饰符会在输入框有 `type="number"` 时自动应用。

`.trim`

## 8. 生命周期钩子

当调用 `onMounted` 时，Vue 会自动将注册的回调函数与当前活动组件实例相关联。这就要求这些钩子在组件设置时同步注册。例如请不要这样做：

```js
setTimeout(() => {
  onMounted(() => {
    // 这将不会正常工作
  })
}, 100)
```

请注意，这并不意味着对 `onMounted` 的调用必须放在 `setup()` 或 `<script setup>` 内的词法环境下。`onMounted()` 也可以在一个外部函数中调用，只要调用栈是同步的，且最终起源自 `setup()`。

[生命周期图示](https://staging-cn.vuejs.org/assets/lifecycle.16e4c08e.png)

[组合式 API：生命周期钩子](https://staging-cn.vuejs.org/api/composition-api-lifecycle.html)

## 9. 侦听器

都是要记的，直接看原文吧。

[侦听器](https://staging-cn.vuejs.org/guide/essentials/watchers.html#this-watch)

​	

## 10. 模板 ref


为了通过组合式 API 获得该模板 ref，我们需要声明一个同名的 ref：

```vue
<script setup>
import { ref, onMounted } from 'vue'

// 声明一个 ref 来存放该元素的引用
// 必须和模板 ref 同名
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>
```

**函数型 ref**

除了使用字符串值作名字，`ref` attribute 还可以绑定为一个函数，会在每次组件更新时都被调用。函数接受该元素引用作为第一个参数：

```
<input :ref="(el) => { /* 将 el 分配给 property 或 ref */ }">
```

如果你正在使用一个动态的 `:ref` 绑定，我们也可以传一个函数。当元素卸载时，这个 `el` 参数会是 `null`。你当然也可以使用一个方法而不是内联函数。



**组件上的 ref**

`ref` 也可以被用在一个子组件上。此时 ref 中引用的是组件实例。

有一个例外的情况，使用了 `<script setup>` 的组件是**默认私有**的：一个父组件无法访问到一个使用了 `<script setup>` 的子组件中的任何东西，除非子组件在其中通过 `defineExpose` 宏显式暴露：

```vue
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

defineExpose({
  a,
  b
})
</script>
```



## 11. 组件基础

`defineProps`

`defineEmits`

**动态组件**

有的需求会想要在两个组件间来回切换，比如 Tab 界面：

[在 Playground 中查看示例](https://sfc.vuejs.org/#eyJBcHAudnVlIjoiPHNjcmlwdCBzZXR1cD5cbmltcG9ydCBIb21lIGZyb20gJy4vSG9tZS52dWUnXG5pbXBvcnQgUG9zdHMgZnJvbSAnLi9Qb3N0cy52dWUnXG5pbXBvcnQgQXJjaGl2ZSBmcm9tICcuL0FyY2hpdmUudnVlJ1xuaW1wb3J0IHsgcmVmIH0gZnJvbSAndnVlJ1xuIFxuY29uc3QgY3VycmVudFRhYiA9IHJlZignSG9tZScpXG5cbmNvbnN0IHRhYnMgPSB7XG4gIEhvbWUsXG4gIFBvc3RzLFxuICBBcmNoaXZlXG59XG48L3NjcmlwdD5cblxuPHRlbXBsYXRlPlxuICA8ZGl2IGNsYXNzPVwiZGVtb1wiPlxuICAgIDxidXR0b25cbiAgICAgICB2LWZvcj1cIihfLCB0YWIpIGluIHRhYnNcIlxuICAgICAgIDprZXk9XCJ0YWJcIlxuICAgICAgIDpjbGFzcz1cIlsndGFiLWJ1dHRvbicsIHsgYWN0aXZlOiBjdXJyZW50VGFiID09PSB0YWIgfV1cIlxuICAgICAgIEBjbGljaz1cImN1cnJlbnRUYWIgPSB0YWJcIlxuICAgICA+XG4gICAgICB7eyB0YWIgfX1cbiAgICA8L2J1dHRvbj5cblx0ICA8Y29tcG9uZW50IDppcz1cInRhYnNbY3VycmVudFRhYl1cIiBjbGFzcz1cInRhYlwiPjwvY29tcG9uZW50PlxuICA8L2Rpdj5cbjwvdGVtcGxhdGU+XG5cbjxzdHlsZT5cbi5kZW1vIHtcbiAgZm9udC1mYW1pbHk6IHNhbnMtc2VyaWY7XG4gIGJvcmRlcjogMXB4IHNvbGlkICNlZWU7XG4gIGJvcmRlci1yYWRpdXM6IDJweDtcbiAgcGFkZGluZzogMjBweCAzMHB4O1xuICBtYXJnaW4tdG9wOiAxZW07XG4gIG1hcmdpbi1ib3R0b206IDQwcHg7XG4gIHVzZXItc2VsZWN0OiBub25lO1xuICBvdmVyZmxvdy14OiBhdXRvO1xufVxuXG4udGFiLWJ1dHRvbiB7XG4gIHBhZGRpbmc6IDZweCAxMHB4O1xuICBib3JkZXItdG9wLWxlZnQtcmFkaXVzOiAzcHg7XG4gIGJvcmRlci10b3AtcmlnaHQtcmFkaXVzOiAzcHg7XG4gIGJvcmRlcjogMXB4IHNvbGlkICNjY2M7XG4gIGN1cnNvcjogcG9pbnRlcjtcbiAgYmFja2dyb3VuZDogI2YwZjBmMDtcbiAgbWFyZ2luLWJvdHRvbTogLTFweDtcbiAgbWFyZ2luLXJpZ2h0OiAtMXB4O1xufVxuLnRhYi1idXR0b246aG92ZXIge1xuICBiYWNrZ3JvdW5kOiAjZTBlMGUwO1xufVxuLnRhYi1idXR0b24uYWN0aXZlIHtcbiAgYmFja2dyb3VuZDogI2UwZTBlMDtcbn1cbi50YWIge1xuICBib3JkZXI6IDFweCBzb2xpZCAjY2NjO1xuICBwYWRkaW5nOiAxMHB4O1xufVxuPC9zdHlsZT4iLCJpbXBvcnQtbWFwLmpzb24iOiJ7XG4gIFwiaW1wb3J0c1wiOiB7XG4gICAgXCJ2dWVcIjogXCJodHRwczovL3NmYy52dWVqcy5vcmcvdnVlLnJ1bnRpbWUuZXNtLWJyb3dzZXIuanNcIlxuICB9XG59IiwiSG9tZS52dWUiOiI8dGVtcGxhdGU+XG4gIDxkaXYgY2xhc3M9XCJ0YWJcIj5cbiAgICBIb21lIGNvbXBvbmVudFxuICA8L2Rpdj5cbjwvdGVtcGxhdGU+IiwiUG9zdHMudnVlIjoiPHRlbXBsYXRlPlxuICA8ZGl2IGNsYXNzPVwidGFiXCI+XG4gICAgUG9zdHMgY29tcG9uZW50XG4gIDwvZGl2PlxuPC90ZW1wbGF0ZT4iLCJBcmNoaXZlLnZ1ZSI6Ijx0ZW1wbGF0ZT5cbiAgPGRpdiBjbGFzcz1cInRhYlwiPlxuICAgIEFyY2hpdmUgY29tcG9uZW50XG4gIDwvZGl2PlxuPC90ZW1wbGF0ZT4ifQ==)

上面的例子是通过 Vue 的 `<component>` 元素和特殊的 `is` attribute 实现的：

```
<!-- currentTab 改变时组件也改变 -->
<component :is="tabs[currentTab]"></component>
```

在上面的例子中，被传给 `:is` 的值可以是以下几种：

- 被注册的组件名
- 导入的组件对象

你也可以使用 `is` attribute 来创建一般的 HTML 元素。

当使用 `<component :is="...">` 来在多个组件间作切换时，组件会在被切换掉后卸载。我们可以通过 [`KeepAlive` 组件](https://staging-cn.vuejs.org/guide/built-ins/keep-alive.html)强制不活跃的组件仍然保持“存活”的状态。



[DOM 模板解析注意事项](https://staging-cn.vuejs.org/guide/essentials/component-basics.html#dom-template-parsing-caveats)



# 2. 深入组件



## 1. 组件注册

* 全局注册：

  ```js
  import MyComponent from './App.vue'
  
  app.component('MyComponent', MyComponent)
  ```

  `app.component()` 方法可以被链式调用：

  ```js
  app
    .component('ComponentA', ComponentA)
    .component('ComponentB', ComponentB)
    .component('ComponentC', ComponentC)
  ```

* 组件名格式

​	在整个指引中，我们都使用 PascalCase 作为组件名的注册格式，这是因为：

1. PascalCase 是合法的 JavaScript 标识符。这使得在 JavaScript 中导入和注册组件都很容易，同时 IDE 也能提供较好的自动补全。
2. `<PascalCase />` 在模板中更明显地表明了这是一个 Vue 组件，而不是原生 HTML 元素。同时也能够将 Vue 组件和自定义元素 (web components) 区分开来。
3. 幸运的是，Vue 支持将使用 kebab-case 的标签解析为使用 PascalCase 注册的组件。这意味着一个以 `MyComponent` 为名注册的组件，在模板中可以通过 `<MyComponent>` 或 `<my-component>` 引用。

## 2. Props

```
const post = {
  id: 1,
  title: 'My Journey with Vue'
}
```

以及下面的模板：

```
<BlogPost v-bind="post" />
```

而这实际上等价于：

```
<BlogPost :id="post.id" :title="post.title" />
```

**类型校验**

```js
 // 函数类型的默认值
  propG: {
    type: Function,
    // 不像对象或数组的默认，这不是一个工厂函数。这会是一个用来作为默认值的函数
    default() {
      return 'Default function'
    }
  }
```

**TIP：**

> `defineProps()` 宏中的参数**不可以访问 `<script setup>` 中定义的其他变量**，因为在编译时整个表达式都会被移到外部的函数中。

注意：

* 如果声明了 `default` 值，那么在 prop 的值被解析为 `undefined` 时，无论 prop 是未被传递还是显式指明的 `undefined`，都会改为 `default` 值。

## 3. 事件

如果一个原生事件的名字 (例如 `click`) 被定义在 `emits` 选项中，则监听器只会监听组件触发的 `click` 事件而不会再响应原生的 `click` 事件。



**事件校验**

和对 prop 添加类型校验的方式类似，所有触发的事件也可以使用对象形式来描述。

要为事件添加校验，那么事件可以被赋值为一个函数，接受的参数就是抛出事件时传入 `emit` 的内容，返回一个布尔值来表明事件是否合法。

```js
<script setup>
const emit = defineEmits({
  // 没有校验
  click: null,

  // 校验 submit 事件
  submit: ({ email, password }) => {
    if (email && password) {
      return true
    } else {
      console.warn('Invalid submit event payload!')
      return false
    }
  }
})

function submitForm(email, password) {
  emit('submit', { email, password })
}
</script>
```





另一种在组件内实现 `v-model` 的方式是使用一个可写的 `computed` property ，给出 getter 和 setter。`get` 方法需返回 `modelValue` property 而 `set` 方法需触发相应的事件：

```js
<!-- CustomInput.vue -->
<script setup>
import { computed } from 'vue'

const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])

const value = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emit('update:modelValue', value)
  }
})
</script>

<template>
  <input v-model="value" />
</template>
```



**多个 v-model**

默认情况下，v-model 使用 modleValue 和 update:modelValue 来实现绑定，我们还可以给 v-model 传递参数来更改这些名字：

```vue
<MyComp v-model:title="bookTitle" />
```

在上面代码中，子组件应该有一个 title prop，并在需要时，向父组件发射 `update:title` 事件。

根据上述情况，就可以利用 v-model 的传参实现多个 v-model 绑定了

```vue
<MyComp v-model="value" v-model:title="bookTitle" v-model:anthor="bookAnthor" />
```

**v-model 的自定义修饰符**

v-model 有一些内置的修饰符，如 .trim .number .lazy

我们也可以自定义一些修饰符，如 capitalize ，它会自动将 v-model 绑定的字符串的第一个字母大写

```vue
<MyComponent v-model.capitalize="myText" />
```

```vue
<script setup>
const props = defineProps({
  modelValue: String,
  modelModifiers: { default: () => ({}) }
})

const emit = defineEmits(['update:modelValue'])

function emitValue(e) {
  let value = e.target.value
  if (props.modelModifiers.capitalize) {
    value = value.charAt(0).toUpperCase() + value.slice(1)
  }
  emit('update:modelValue', value)
}
</script>

<template>
  <input type="text" :value="modelValue" @input="emitValue" />
</template>
```

又或者

```vue
<MyComponent v-model:title.capitalize="myText">
```

```js
const props = defineProps(['title', 'titleModifiers'])
defineEmits(['update:title'])

console.log(props.titleModifiers) // { capitalize: true }
```

## 4. 透传 Attribute

当一个组件以单个元素为根作渲染时，透传的 attribute 会自动添加到根元素的 attribute 中。

如果一个子组件的根元素已经有了 `class` 或 `style` attribute，它会和从父组件上继承的值合并。

同样的规则也适用于 `v-on` 事件监听器：

```
<MyButton @click="onClick" />
```

监听器 `click` 会被添加到 `<MyButton>` 的根元素，即那个原生的 `<button>` 元素之上。当原

生的 `<button>` 被点击，会触发父组件的 `onClick` 方法。如果原生 `button` 元素已经通过 

`v-on` 绑定了一个事件监听器，则这些监听器都会被触发。

**禁用 Attribute 继承**

`inheritAttrs: false`

如果你使用了 `<script setup>`，你需要一个额外的 `<script>` 块来书写这个选项声明：

```vue
<script>
// 使用一个简单的 <script> to declare options
export default {
  inheritAttrs: false
}
</script>

<script setup>
// ...setup 部分逻辑
</script>
```

有几点需要注意：

- 和 props 有所不同，透传 attributes 在 JavaScript 中保留了它们原始的大小写，所以像 `foo-bar` 这样的一个 attribute 需要通过 `$attrs['foo-bar']` 来访问。
- 像 `@click` 这样的一个 `v-on` 事件监听器将在此对象下被暴露为一个函数 `$attrs.onClick`。



**多根节点的 Attribute 继承**

和单根节点组件有所不同，有着多个根节点的组件没有自动 attribute 透传行为。如果 `$attrs` 

没有被显式绑定，将会抛出一个运行时警告。

如果需要，你可以在 `<script setup>` 中使用 `useAttrs()` API 来访问一个组件的所有透传 attribute：

```vue
<script setup>
import { useAttrs } from 'vue'

const attrs = useAttrs()
</script>
```

```vue
export default {
  setup(props, ctx) {
    // 透传 attribute 被暴露为 ctx.attrs
    console.log(ctx.attrs)
  }
}
```

**TIP**: 需要注意的是，虽然这里的 `attrs` 对象总是反映为最新的透传 attribute，但它并不是响应式的 (考虑到性能因素)。你不能通过侦听器去监听它的变化。如果你需要响应性，可以使用 prop。或者你也可以使用 `onUpdated()` 使得在每次更新时结合最新的 `attrs` 执行副作用。

## 5. 插槽

**动态插槽名**

[动态指令参数](https://staging-cn.vuejs.org/guide/essentials/template-syntax.html#dynamic-arguments)在 `v-slot` 上也是有效的，即可以定义下面这样的动态插槽名：

```vue
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>

  <!-- 缩写为 -->
  <template #[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

注意这里的表达式和动态指令参数受相同的[语法限制](https://staging-cn.vuejs.org/guide/essentials/template-syntax.html#directives)。



可以通过 v-bind 来传递props

```
<ul>
  <li v-for="item in items">
    <slot name="item" v-bind="item"></slot>
  </li>
</ul>
```



**无渲染组件**

上面的 `<FancyList>` 案例同时封装了可重用的逻辑 (数据获取、分页等) 和视图输出，但也将部分视图输出通过作用域插槽交给了消费者组件来管理。

如果我们将这个概念拓展一下，可以想象的是，一些组件可能只包括了逻辑而不需要自己渲染内容，视图输出通过作用域插槽全权交给了消费者组件。我们将这种类型的组件称为**无渲染组件**。

这里有一个无渲染组件的例子，一个封装了追踪当前鼠标位置逻辑的组件：

```
<MouseTracker v-slot="{ x, y }">
  Mouse is at: {{ x }}, {{ y }}
</MouseTracker>
```

[在 Playground 中尝试一下](https://sfc.vuejs.org/#eyJBcHAudnVlIjoiPHNjcmlwdCBzZXR1cD5cbmltcG9ydCBNb3VzZVRyYWNrZXIgZnJvbSAnLi9Nb3VzZVRyYWNrZXIudnVlJ1xuPC9zY3JpcHQ+XG5cbjx0ZW1wbGF0ZT5cblx0PE1vdXNlVHJhY2tlciB2LXNsb3Q9XCJ7IHgsIHkgfVwiPlxuICBcdE1vdXNlIGlzIGF0OiB7eyB4IH19LCB7eyB5IH19XG5cdDwvTW91c2VUcmFja2VyPlxuPC90ZW1wbGF0ZT4iLCJpbXBvcnQtbWFwLmpzb24iOiJ7XG4gIFwiaW1wb3J0c1wiOiB7XG4gICAgXCJ2dWVcIjogXCJodHRwczovL3NmYy52dWVqcy5vcmcvdnVlLnJ1bnRpbWUuZXNtLWJyb3dzZXIuanNcIlxuICB9XG59IiwiTW91c2VUcmFja2VyLnZ1ZSI6IjxzY3JpcHQgc2V0dXA+XG5pbXBvcnQgeyByZWYsIG9uTW91bnRlZCwgb25Vbm1vdW50ZWQgfSBmcm9tICd2dWUnXG4gIFxuY29uc3QgeCA9IHJlZigwKVxuY29uc3QgeSA9IHJlZigwKVxuXG5jb25zdCB1cGRhdGUgPSBlID0+IHtcbiAgeC52YWx1ZSA9IGUucGFnZVhcbiAgeS52YWx1ZSA9IGUucGFnZVlcbn1cblxub25Nb3VudGVkKCgpID0+IHdpbmRvdy5hZGRFdmVudExpc3RlbmVyKCdtb3VzZW1vdmUnLCB1cGRhdGUpKVxub25Vbm1vdW50ZWQoKCkgPT4gd2luZG93LnJlbW92ZUV2ZW50TGlzdGVuZXIoJ21vdXNlbW92ZScsIHVwZGF0ZSkpXG48L3NjcmlwdD5cblxuPHRlbXBsYXRlPlxuICA8c2xvdCA6eD1cInhcIiA6eT1cInlcIi8+XG48L3RlbXBsYXRlPiJ9)

虽然这个模式很有趣，但大部分能用无渲染组件实现的功能都可以通过组合式 API 以另一种更高效的方式实现，并且还不会带来额外组件嵌套的开销。之后我们会在[组合式函数](https://staging-cn.vuejs.org/guide/reusability/composables.html)一章中介绍如何更高效地实现追踪鼠标位置的功能。

## 6. 依赖注入

```
<script setup>
import { provide } from 'vue'

provide(/* 注入名 */ 'message', /* 值 */ 'hello!')
</script>
```

第二个参数是供给的值，值可以是任意类型，包括响应式的状态，比如一个 ref：

```
import { ref, provide } from 'vue'

const count = ref(0)
provide('key', count)
```

供给的响应式状态使后代组件可以由此和供给者建立响应式的联系。



**应用层 Provide**

除了供给一个组件的数据，我们还可以在整个应用层面做供给：

```
import { createApp } from 'vue'

const app = createApp({})

app.provide(/* 注入名 */ 'message', /* 值 */ 'hello!')
```

应用级的供给在应用的所有组件中都可以注入。这在你编写[插件](https://staging-cn.vuejs.org/guide/reusability/plugins.html)时会特别有用，因为插件一般都不会使用组件形式来供给值。



**注入**

```vue
<script setup>
import { inject } from 'vue'

const message = inject('message')
</script>
```

如果供给的值是一个 ref，注入进来的就是它本身，而**不会**自动解包。这使得被注入的组件保持了和供给者的

响应性链接。

**默认值**

```
// 如果没有祖先组件提供 "message"
// `value` 会是 "这是默认值"
const value = inject('message', '这是默认值')
```

**配合响应性**

当使用响应式 `provide`/`inject` 值时，**建议尽可能将任何对响应式状态的变更都保持在 \*provider\* 内部**。 

这样可以确保 `provide` 的状态和变更操作都在同一个组件内，使其更容易维护。

有的时候，我们可能需要在 `injector` 组件中更改数据。在这种情况下，我们推荐在 `provider` 组件内提供

一个更改数据方法：

```
<!-- 在 provider 组件内 -->
<script setup>
import { provide, ref } from 'vue'

const location = ref('North Pole')

function updateLocation() {
  location.value = 'South Pole'
}

provide('location', {
  location,
  updateLocation
})
</script>
```



最后，如果你想确保从 `provide` 传过来的数据不能被 `injector` 的组件更改，你可以使用[`readonly()`](https://staging-cn.vuejs.org/api/reactivity-core.html#readonly) 来包装提供的值。

```
<script setup>
import { ref, provide, readonly } from 'vue'

const count = ref(0)
provide('read-only-count', readonly(count))
</script>
```



**使用 Symbol 作注入名**

至此，我们已经了解了如何使用字符串作为注入名。但如果你正在构建大型的应用程序，包含非常多的依赖供给，或者你正在编写提供给其他开发者使用的组件库，建议最好使用 Symbol 来作为注入名以避免潜在的冲突。

建议在一个单独的文件中导出这些注入名 Symbol：

```
// keys.js
export const myInjectionKey = Symbol()
// 在供给方组件中
import { provide } from 'vue'
import { myInjectionKey } from './keys.js'

provide(myInjectionKey, { /*
  要供给的数据
*/ });
// 注入方组件
import { inject } from 'vue'
import { myInjectionKey } from './keys.js'

const injected = inject(myInjectionKey)
```

 

## 7. 异步组件

```
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)
```

加载出错时的解决方案

```
const AsyncComp = defineAsyncComponent({
  // 加载函数
  loader: () => import('./Foo.vue'),

  // 加载异步组件时使用的组件
  loadingComponent: LoadingComponent,
  // 展示加载组件前的延迟时间，默认为 200ms
  delay: 200,

  // 加载失败后展示的组件
  errorComponent: ErrorComponent,
  // 如果提供了一个 timeout 时间限制，并超时了
  // 也会显示这里配置的报错组件，默认值是：Infinity
  timeout: 3000
})
```







# 3. 可重用性

## 1. 组合式函数

**无状态的逻辑函数：** 例如为了在不同地方格式化时间而抽取一个可复用的函数。这个格式化函数封装了**无状态的逻辑**：它在接收一些输入后立刻返回所期望的输出。如 [lodash](https://lodash.com/) 。

**有状态的逻辑函数：** 有状态逻辑负责管理会随时变化的状态。例如跟踪当前鼠标在页面中的位置。但是，如果我们想在多个组件中复用这个相同的逻辑呢？我们可以把这个逻辑以一个组合式函数的形式提取到外部文件中：

*原来是 Vueuse 啊*

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



```vue
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```



一个组合式函数可以调用一个或多个其他的组合式函数。

```js
// event.js
import { onMounted, onUnmounted } from 'vue'

export function useEventListener(target, event, callback) {
  // 如果你想的话，
  // 也可以用字符串形式的 CSS 选择器来寻找目标 DOM 元素
  onMounted(() => target.addEventListener(event, callback))
  onUnmounted(() => target.removeEventListener(event, callback))
}
```

现在，`useMouse()` 可以被简化为：

```js
// mouse.js
import { ref } from 'vue'
import { useEventListener } from './event'

export function useMouse() {
  const x = ref(0)
  const y = ref(0)

  useEventListener(window, 'mousemove', (event) => {
    x.value = event.pageX
    y.value = event.pageY
  })

  return { x, y }
}
```



组合式函数也可以接收参数，来处理一些逻辑，如 url 请求，若参数是一个静态的，则只执行一次

若需要组合式函数可以根据 url 的变化来触发请求，可以使用一个 ref 作为参数，结合 watchEffect 来实现，如下：

```js
// fetch.js
import { ref, isRef, unref, watchEffect } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)

  function doFetch() {
    // 在请求之前重设状态...
    data.value = null
    error.value = null
    // unref() 解包可能为 ref 的值
    fetch(unref(url))
      .then((res) => res.json())
      .then((json) => (data.value = json))
      .catch((err) => (error.value = err))
  }

  if (isRef(url)) {
    // 若输入的 URL 是一个 ref，那么启动一个响应式的请求
    watchEffect(doFetch)
  } else {
    // 否则只请求一次
    // 避免监听器的额外开销
    doFetch()
  }

  return { data, error }
}
```



**约定**

**命名：**组合式函数约定用驼峰命名法命名，并以“use”作为开头。



**输入参数：**

尽管其响应性不依赖 ref，组合式函数仍可接收 ref 参数。如果编写的组合式函数会被其他开发者使用，你最好在处理输入参数时兼容 ref 而不只是原始的值。[`unref()`](https://staging-cn.vuejs.org/api/reactivity-utilities.html#unref) 工具函数会对此非常有帮助：

```js
import { unref } from 'vue'

function useFeature(maybeRef) {
  // 若 maybeRef 确实是一个 ref，它的 .value 会被返回
  // 否则，maybeRef 会被原样返回
  const value = unref(maybeRef)
}
```

如果你的组合式函数在接收 ref 为参数时会产生响应式 effect，请确保使用 `watch()` 显式地监听此 ref，或者在 `watchEffect()` 中调用 `unref()` 来进行正确的追踪。



**返回值：**推荐的约定是组合式函数始终返回一个 ref 对象不是 `reactive`，这样该函数在组件中解构之后仍可以保持响应性。`reactive`会导致在对象解构过程中丢失与组合式函数内状态的响应性连接。

可以将要返回的对象用 `reactive()` 包装:

```js
const mouse = reactive(useMouse())
// mouse.x 链接到了原来的 x ref
console.log(mouse.x)
```



**副作用**

确保在 `onUnmounted()` 时清理副作用。举个例子，如果一个组合式函数设置了一个事件监听器，它就应该在 `onUnmounted()` 中被移除 (就像我们在 `useMouse()` 示例中看到的一样)。当然也可以像之前的`useEventListener()` 示例那样，使用一个组合式函数来自动帮你做这些事。



**使用限制**

组合式函数在 `<script setup>` 或 `setup()` 钩子中，应始终被**同步地**调用。在某些场景下，你也可以在像 `onMounted()` 这样的生命周期钩子中使用他们。

这些是 Vue 得以确定当前活跃的组件实例的条件。有能力对活跃的组件实例进行访问是必要的，以便：

1. 可以在组合式函数中注册生命周期钩子
2. 计算属性和监听器可以连接到当前组件实例，以便在组件卸载时处理掉。



**在选项 API 中使用**

如果你正在使用选项式 API，组合式函数必须在 `setup()` 中调用。且其返回的绑定必须在 `setup()` 中返回，以便暴露给 `this` 及其模板

```js
import { useMouse } from './mouse.js'
import { useFetch } from './fetch.js'

export default {
  setup() {
    const { x, y } = useMouse()
    const { data, error } = useFetch('...')
    return { x, y, data, error }
  },
  mounted() {
    // setup() 暴露的 property 可以在通过 `this` 访问到
    console.log(this.x)
  }
  // ...其他选项
}
```



**和 mixin 比较，mixin 的短板：**

1. property 来源不清晰
2. 命名空间冲突
3. 隐式的跨 mixin 交流：多个 mixin 需要依赖共享的 property 键名来进行相互作用，这使得它们隐性地耦合在一起。而一个组合式函数的返回值可以作为另一个组合式函数的参数被传入，像普通函数那样。

**基于上述理由，我们不再推荐在 Vue 3 中继续使用 mixin。保留该功能只是为了项目迁移的需求和照顾熟悉它的用户。**



## 2. 自定义指令

在 `<script setup>` 中，任何以 `v` 开头的驼峰式命名的变量都可以被用作一个自定义指令。

如果不使用 `<script setup>`，自定义指令可以通过 `directives` 选项注册

**指令钩子**

```js
const myDirective = {
  // 在绑定元素的 attribute 前
  // 或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
  },
  // 在元素被插入到 DOM 前调用
  beforeMount() {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted() {},
  // 绑定元素的父组件更新前调用
  beforeUpdate() {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated() {},
  // 绑定元素的父组件卸载前调用
  beforeUnmount() {},
  // 绑定元素的父组件卸载后调用
  unmounted() {}
  }
}
```



**钩子参数**

指令的钩子会传递以下几种参数：

- `el`：指令绑定到的元素。这可以用于直接操作 DOM。
- `binding`：一个对象，包含以下 property。
  - `value`：传递给指令的值。例如在 `v-my-directive="1 + 1"` 中，值是 `2`。
  - `oldValue`：之前的值，仅在 `beforeUpdate` 和 `updated` 中可用。无论值是否更改，它都可用。
  - `arg`：传递给指令的参数 (如果有的话)。例如在 `v-my-directive:foo` 中，参数是 `"foo"`。
  - `modifiers`：一个包含修饰符的对象 (如果有的话)。例如在 `v-my-directive.foo.bar` 中，修饰符对象是 `{ foo: true, bar: true }`。
  - `instance`：使用该指令的组件实例。
  - `dir`：指令的定义对象。
- `vnode`：代表绑定元素的底层 VNode。
- `prevNode`：之前的渲染中代表指令所绑定元素的 VNode。仅在 `beforeUpdate` 和 `updated` 钩子中可用。



**在组件上使用**

当在组件上使用自定义指令时，它会始终应用于组件的根节点，和[透传 attributes](https://staging-cn.vuejs.org/guide/components/attrs.html) 类似。

需要注意的是组件可能含有多个根节点。当应用到一个多根组件时，指令将会被忽略且抛出一个警告。和 attribute 不同，指令不能通过 `v-bind="$attrs"` 来传递给一个不同的元素。总而言之，**不**推荐在组件上使用自定义指令。

## 3. 插件

`app.use(plugin)`

感觉插件好强大啊，该如何更好的使用插件呢？



# 4. 内置组件



## 1. Transition·过渡

它可以将进入和离开动画应用到通过默认插槽传递给它的元素或组件上。进入或离开可以由以下的条件之一触发：

- 由 `v-if` 所带来的条件渲染
- 由 `v-show` 所带来的条件显示
- 由特殊元素 `<component>` 切换的动态组件

> TIP
>
> `<Transition>` 仅支持单个元素或组件作为其插槽内容。如果内容是一个组件，这个组件必须仅有一个根元素。

[官方文档](https://staging-cn.vuejs.org/guide/built-ins/transition.html)

## 2. TransitionGroup·过渡组

```
/* 确保将离开的元素从布局流中删除
  以便能够正确地计算移动的动画。 */
.list-leave-active {
  position: absolute;
}
```

[看文档吧](https://staging-cn.vuejs.org/guide/built-ins/transition-group.html)



## 3. KeepAlive

`<KeepAlive>` 是一个内置组件，使我们可以在动态切换多个组件时视情况缓存组件实例。



它会根据组件的 [`name`](https://staging-cn.vuejs.org/api/options-misc.html#name) 选项进行匹配，所以组件如果想要条件性地被 `KeepAlive` 缓存，就必须显式声明一个 `name` 选项。



我们可以通过传入 `max` prop 来限制可被缓存的最大组件实例数。`<KeepAlive>` 的行为在指定了 `max` 后类似一个 [LRU 缓存](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU))：如果缓存的实例数量即将超过指定的那个最大数量，则最久没有被访问的缓存实例将被销毁，以便为新的实例腾出空间。



**缓存实例的生命周期**

当一个组件实例从 DOM 上移除但因为被 `<KeepAlive>` 缓存而仍作为组件树的一部分时，它将变为**不活跃**状态而不是被卸载。当一个组件实例作为缓存树的一部分插入到 DOM 中时，它将重新**被激活**。

一个持续存在的组件可以通过 [`onActivated()`](https://staging-cn.vuejs.org/api/composition-api-lifecycle.html#onactivated) 和 [`onDeactivated()`](https://staging-cn.vuejs.org/api/composition-api-lifecycle.html#ondeactivated) 注册相应的两个状态的生命周期钩子：

```
<script setup>
import { onActivated, onDeactivated } from 'vue'

onActivated(() => {
  // 调用时机为首次挂载
  // 以及每次从缓存中被重新插入时
})

onDeactivated(() => {
  // 在从 DOM 上移除、进入缓存
  // 以及组件卸载时调用
})
</script>
```

请注意：

- `onActivated` 在组件挂载时也会调用，并且 `onDeactivated` 在组件卸载时也会调用。
- 这两个钩子不仅适用于 `<KeepAlive>` 缓存的根组件，也适用于缓存树中的后代组件。



## 4. Teleport

```
<button @click="open = true">Open Modal</button>

<Teleport to="body">
  <div v-if="open" class="modal">
    <p>Hello from the modal!</p>
    <button @click="open = false">Close</button>
  </div>
</Teleport>
```

> TIP
>
> `<Teleport>` 挂载时，传送门的 `to` 目标必须是已经存在于 DOM 之中。理想情况下，这应该是整个 Vue 应用程序之外的一个元素。如果目标是由 Vue 呈现的另一个元素，你需要确保在 `<Teleport>` 之前挂载该元素。


**禁用teleport**

```
<Teleport :disabled="isMobile">
  ...
</Teleport>
```



**同一目标上多个传送门**

一个常见的应用场景就是写一个可重用的 `<Modal>` 组件，可能同时存在多个实例。对于此类场景，多个 `<teleport>` 组件可以将其内容挂载在同一个目标元素上，而顺序就是简单的顺次追加，后挂载的将排在目标元素下更后面的位置上。



## 5. Suspense

[Suspense](https://staging-cn.vuejs.org/guide/built-ins/suspense.html)



# 5. 升级规模

## 1. Vue-router

## 2. Pinia

## 3. vite

## 4. vitest

## 5. SSR



# 6. 最佳实践

## 1. 性能

看文档吧





# 7. typescript

## 1. 





# 8. 进阶主题

## 1. 深入响应式系统

**组件调试钩子**

我们可以在一个组件渲染时调试查看哪些依赖正在被使用，以及使用 `onRenderTracked` 和 `onRenderTriggered` 生命周期钩子来确定哪个依赖正在触发更新。这些钩子都会收到一个调试事件，其中包含了所需依赖的信息。推荐在回调中放置一个 `debugger` 语句，使你可以在开发者工具中交互式地查看依赖

```js
<script setup>
import { onRenderTracked, onRenderTriggered } from 'vue'

onRenderTracked((event) => {
  debugger
})

onRenderTriggered((event) => {
  debugger
})
</script>
```



```js
type DebuggerEvent = {
  effect: ReactiveEffect
  target: object
  type:
    | TrackOpTypes /* 'get' | 'has' | 'iterate' */
    | TriggerOpTypes /* 'set' | 'add' | 'delete' | 'clear' */
  key: any
  newValue?: any
  oldValue?: any
  oldTarget?: Map<any, any> | Set<any>
}
```



**计算属性调试**

我们可以向 `computed()` 传入第二个参数，是一个包含了 `onTrack` 和 `onTrigger` 两个回调函数的对象：

- `onTrack` 将在响应属性或引用作为依赖项被跟踪时被调用。
- `onTrigger` 将在侦听器回调被依赖项的变更触发时被调用。

```js
const plusOne = computed(() => count.value + 1, {
  onTrack(e) {
    // 当 count.value 被追踪为依赖时触发
    debugger
  },
  onTrigger(e) {
    // 当 count.value 被更改时触发
    debugger
  }
})

// 访问 plusOne，会触发 onTrack
console.log(plusOne.value)

// 更改 count.value，应该会触发 onTrigger
count.value++
```

**侦听器调试**

和 `computed()` 类似，侦听器也支持 `onTrack` 和 `onTrigger` 选项：

```js
watch(source, callback, {
  onTrack(e) {
    debugger
  },
  onTrigger(e) {
    debugger
  }
})

watchEffect(callback, {
  onTrack(e) {
    debugger
  },
  onTrigger(e) {
    debugger
  }
})
```

**浅层响应式**

**shallowRef**

一个浅层的 ref 中只有它的 `.value` 属性本身被访问时才是有响应性的，而不关心它内部的值。当外部状态改变时，替换此 ref 的 `.value` 才会触发更新。

```js
const state = shallowRef({ count: 1 })

// 不会触发更改
state.value.count = 2

// 会触发更改
state.value = { count: 2 }
```

**shallowReactive**

和 `reactive()`不同，这里不会有深层次转换：一个浅层响应式对象里只有根级别的属性是响应式的。属性的值会被原样存储和暴露，这也意味着属性为 ref 的值 **不会** 被自动解包了。

```js
const state = shallowReactive({
  foo: 1,
  nested: {
    bar: 2
  }
})

// 更改状态自身的属性是响应式的
state.foo++

// ...但下层嵌套对象不会被转为响应式
isReactive(state.nested) // false

// 不是响应式的
state.nested.bar++
```

## 2. 渲染机制

看文档吧

