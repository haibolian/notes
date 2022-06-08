# Element-plus 源码学习系列 - Input

## 1. ts - as const

`as const` 可以使对象内部属性变成 readonly，属性的属性也会变成 readonly

```ts
const PENDANT_MAP = {
  suffix: 'append',
  prefix: 'prepend',
} as const
/**
const PENDANT_MAP: {
    readonly suffix: "append";
    readonly prefix: "prepend";
}
**/

const PENDANT_MAP = {
  suffix: 'append',
  prefix: 'prepend',
  test: {
    test_child: ''
  }
} as const
/**
const PENDANT_MAP: {
    readonly suffix: "append";
    readonly prefix: "prepend";
    readonly test: {
        readonly test_child: "";
    };
}
**/
```



## 2. 输入法事件

* **`compositionstart`**：开始新的输入合成时会触发 **`compositionstart`** 事件。
* **`compositionend`**：当文本段落的组成完成或取消时，compositionend 事件将被触发
* **`compositionupdate`** ： **`compositionupdate`** 事件触发于字符被输入到一段文字的时候（这些可见字符的输入可能需要一连串的键盘操作、语音识别或者点击输入法的备选词）



## 3. 
