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