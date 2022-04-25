[TOC]

在2.6.0中，Vue将 `slot` 和 `slot-scope` 废弃，但移除。下面学习学习插槽的新用法。

## 1. 基本使用

和之前一样

```vue
<!-- About.vue -->
<about-comp url="/profile">
    Your Profile
</about-comp>

<!-- AboutComp.vue   About的子组件 -->
<template>
  <div>
    <a v-bind:href="url">
      <slot></slot>
    </a>
  </div>
</template>
<script>
export default {
  props:['url']
};
</script>
```

父子组件的传参，只有一个默认插槽，那么在父组件中，子组件标签之间的内容会替代子组件的`slot` 部分。

如果`AboutComp`中没有`slot`，那么在`About`中，`about-comp`标签之间的内容全部被抛弃。

## 2. 编译作用域

如下代码，`child_msg`  是在子组件中定义的数据

```vue
<about-comp url="/profile">
    Your Profile
    {{ child_msg }}
</about-comp>
```

在父组件中想访问子组件中的数据是访问不到的。

记住：

**父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

## 3. 插槽默认值

当父组件使用子组件标签时，不给插槽传值，插槽可以使用默认的值。

```vue
<!-- 父组件 -->
<submit-button></submit-button>

<!-- 子组件 -->
<button type="submit">
  <slot></slot>
</button>
```

如上代码，父组件不给标签写内容，插槽中什么也不会显示。

```vue
<!-- 父组件 -->
<submit-button></submit-button>

<!-- 子组件 -->
<button type="submit">
  <slot>Submit</slot>
</button>
```

如上代码，父组件不给标签写内容，插槽中会显示默认的 Submit 文本。

如果父组件在标签中写了内容，则内容会替换掉默认的 Submit 文本

## 4. 具名插槽

```vue
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

在向具名插槽提供内容的时候，我们可以在一个 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称：

```vue
<about-comp>
    <template v-slot:header>
		<h1>Here might be a page title</h1>
    </template>
    
    <p>And another one.</p>

    <template v-slot:footer>
		<p>Here's some contact info</p>
    </template>
</about-comp>
```

注意，`v-slot` 后面跟的是 **冒号** `：` ，**冒号后面没有引号** ，

**一个不带 `name` 的 `<slot>` 出口会带有隐含的名字“default”。**

```vue
<about-comp>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default>
    <p>helloWorld</p>
  </template>

  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</about-comp>
```

如上，`v-slot:default` 会写进子组件中 `main` 标签中的插槽。而在父组件中，`p` 标签的内容会被抛弃掉，因为已经没有其他插槽供 `p` 标签使用了。

**注意** `v-slot` 只能添加在 `template` 上，( 只有[一种例外情况](https://cn.vuejs.org/v2/guide/components-slots.html#独占默认插槽的缩写语法)) ，这一点和已经废弃的 [`slot` attribute](https://cn.vuejs.org/v2/guide/components-slots.html#废弃了的语法) 不同。

## 5. 作用域插槽

### 5.1 基本使用

作用域插槽可以让插槽内容能够访问子组件中的数据。

当父组件使用子组件标签时，可以直接拿子组件中的数据在标签中进行各种设置。

```vue
<current-user>
  <!-- user是子组件中的数据 -->
  {{ user.firstName }}   
</current-user>
```

然而上述代码不会正常工作，因为只有 `<current-user>` 组件可以访问到 `user` 而我们提供的内容是在父级渲染的。

为了让 `user` 在父级的插槽内容中可用，我们可以将 `user` 作为 `<slot>` 元素的一个 attribute 绑定上去：

```vue
<span>
  <slot :user="user">
    {{ user.lastName }}
  </slot>
</span>
```

绑定在 `<slot>` 元素上的 attribute 被称为**插槽 prop**。现在在父级作用域中，我们可以使用带值的 `v-slot` 来定义我们提供的插槽 prop 的名字，这个`slotProps`名字是任意的。

```vue
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

也可以写成下面这样

```vue
<current-user>
  <template v-slot="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

### 5.2 独占默认插槽缩写语法

当只有默认插槽时，组件的标签才可以被当作插槽的模板来使用。这样我们就可以把 `v-slot` 直接用在组件上：

```vue
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

这种写法还可以更简单。就像假定未指明的内容对应默认插槽一样，不带参数的 `v-slot` 被假定对应默认插槽：

```vue
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

### 5.3 具名插槽和缩写语法

注意默认插槽的缩写语法**不能**和具名插槽混用，因为它会导致作用域不明确：

```vue
<!-- 无效，会导致警告 -->
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
  <template v-slot:other="otherSlotProps">
    slotProps is NOT available here
  </template>
</current-user>
```

### 5.4 多个插槽

Vue官方文档在这一块介绍的太少，只说了一句：

> 只要出现多个插槽，要终为所有的插槽使用完整的基于 `<template>` 的语法。

我觉得官方说的不够准确，应该说要配合具名插槽一起使用，虽然它的代码中带了默认插槽的使用。

下面说一下具体的使用方法：

当出现多个插槽时，

- 首先要保证为所有的插槽使用完整的基于`<template>` 的语法，
- 在子组件中，在需要的地方提供具名插槽，并将需要传给父组件的数据绑定到`<slot>`上去
- 在父组件中，`<template>` 模板中加上 `v-slot:插槽名字=“任意名称”` 

```vue
<!-- 子组件 -->
<div>
  <slot name="baseInfo" :user='user'></slot>
  <br>
  <slot name="otherInfo" :other='other'></slot>
</div>
<script>
export default {
  data(){
    return{
      user:{
        name: 'alin',
        age: 17
      },
      other:{
        like: '运动，跑步',
        tabs: '潇洒，阳光'
      }}}} </script>
<!-- 父组件 -->
<hello-world>
  <template v-slot:baseInfo="firstInfo">
    {{ firstInfo }}
  </template>
  <template v-slot:otherInfo="secondInfo">
    {{ secondInfo }}
  </template>
</hello-world>
```

### 5.5 解构插槽Prop

作用域插槽的内部工作原理是将你的插槽内容包括在一个传入单个参数的函数里：

```
function (slotProps) {
  // 插槽内容
}
```

这意味着 `v-slot` 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式。所以在支持的环境下 ([单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)或[现代浏览器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#浏览器兼容))，你也可以使用 [ES2015 解构](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#解构对象)来传入具体的插槽 prop，如下：

```vue
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```

这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样开启了 prop 重命名等其它可能，例如将 `user` 重命名为 `person`：

```vue
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```

## 6. 动态插槽名

[动态指令参数](https://cn.vuejs.org/v2/guide/syntax.html#动态参数)也可以用在 `v-slot` 上，来定义动态的插槽名：

```vue
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
```

## 7. 具名插槽的缩写

跟 `v-on` 和 `v-bind` 一样，`v-slot` 也有缩写，即把参数之前的所有内容 (`v-slot:`) 替换为字符 `#`。

例如 `v-slot:header` 可以被重写为 `#header`：

```vue
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

然而，和其它指令一样，该缩写只在其有参数的时候才可用。这意味着以下语法是无效的：

```vue
<!-- 这样会触发一个警告 -->
<current-user #="{ user }">
  {{ user.firstName }}
</current-user>
```

如果你希望使用缩写的话，你必须始终以明确插槽名取而代之：

```vue
<current-user #default="{ user }">
  {{ user.firstName }}
</current-user>
```