# Element-plus 源码学习系列 - hooks

## 1. useNamespace

`namespace` 用于自定义类名，如之前 `element-ui` 时的类名诸如 `el-**` 之类的现在可以自定义了。

返回一些方法用来调用生成类名的字符串

```ts
const _bem = (
  namespace: string,   // el
  block: string, // button
  blockSuffix: string, // group
  element: string, // wapper inner
  modifier: string   // 修饰符  success/info/error//  primary / danger
) => {
  let cls = `${namespace}-${block}`  //el-button
  if (blockSuffix) {
    cls += `-${blockSuffix}`
  }
  if (element) {
    cls += `__${element}`
  }
  if (modifier) {
    cls += `--${modifier}`
  }
  return cls
}
```

```ts
const useNamespace = (block: string) => {
  const globalConfig = useGlobalConfig('namespace')
  const namespace = computed(() => globalConfig.value || defaultNamespace)
  const b = (blockSuffix = '') =>
    _bem(unref(namespace), block, blockSuffix, '', '')
  const e = (element?: string) =>
    element ? _bem(unref(namespace), block, '', element, '') : ''
  const m = (modifier?: string) =>
    modifier ? _bem(unref(namespace), block, '', '', modifier) : ''
```

还有 `be`、`bm`、`em`、 `is` 之类的



## 2. useProp

```ts
export const useProp = <T>(name: string): ComputedRef<T | undefined> => {
  const vm = getCurrentInstance()!
  return computed(() => (vm.proxy?.$props as any)[name] ?? undefined)
}
```

通过 `getCurrentInstance` 可以拿到当前实例的 $props，也就可以在非 vue 文件中获取到该实例的 prop。



## 3. useDisabled

```ts
const disabled = useDisabled(computed(() => radioGroup?.disabled))
```

```ts
export const useDisabled = (fallback?: MaybeRef<boolean | undefined>) => {
  const disabled = useProp<boolean>('disabled')
  const form = inject(formContextKey, undefined)
  return computed(
    () => disabled.value || unref(fallback) || form?.disabled || false
  )
}
```

大多数表单控件的 disabled、size 等属性都有很多因素影响，如 form、group、和自己。在 useDisabled 中拿到自己的 disabled 和表单的 disabled，group 的disabled 属性不好统一管理，通过 fallback 将其 disabled 属性ref 值传过来，三者进行判断，最终返回合适的 disabled。

useSize 也是类似。

这样做可以统一管理，不需要每个表单控件重复的判断了。







