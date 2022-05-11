# Element-plus 源码学习系列 - Button

## 1. autofocus

页面加载后，自动聚焦，其他表单控件也可以使用

## 2. 多层控制条件

像 `size`、`disabled`、`type` 受很多因素控制，如组件本身的 `prop`、`全局设置`、`form`。到底使用哪个 size 由 `useSize` 这个 hook 决定

```ts
const _size = useSize(computed(() => buttonGroupContext?.size))
const _disabled = useDisabled()
const _type = computed(() => props.type || buttonGroupContext?.type || '')
```

拿 size 举例：

先把各个控制 size 因素的结果拿出来，拿出来之后通过 || 运算符来决定使用哪个因素的 size。

还可以接受一个 `ignore` 参数来手动控制要忽略的因素

```ts
export const useSize = (
  fallback?: MaybeRef<ComponentSize | undefined>,
  ignore: Partial<Record<'prop' | 'form' | 'formItem' | 'global', boolean>> = {}
) => {
  const emptyRef = ref(undefined)

  const size = ignore.prop ? emptyRef : useProp<ComponentSize>('size')
  const globalConfig = ignore.global ? emptyRef : useGlobalConfig('size')
  const form = ignore.form
    ? { size: undefined }
    : inject(formContextKey, undefined)
  const formItem = ignore.formItem
    ? { size: undefined }
    : inject(formItemContextKey, undefined)

  return computed(
    (): ComponentSize =>
      size.value ||
      unref(fallback) ||
      formItem?.size ||
      form?.size ||
      globalConfig.value ||
      ''
  )
}
```



## 3. 点击事件

```ts
const handleClick = (evt: MouseEvent) => {
  if (props.nativeType === 'reset') {
    form?.resetFields()
  }
  emit('click', evt)
}
```

事件比较简单，只不过之前没有用过 el-button 的 native-type 属性

当 native-type 为 reset 时， 用户可以自动重置表单，不用在手动调用。



