# Storage 事件

当修改 localStorage 或 sessionstorage 时，会触发 storage 事件

storage 事件只会发送给**同源**、而且处于**打开状态**的**其它页面**，而不会发送给触发改变的**页面本身**及处于**关闭状态**的页面

[参考文章](https://www.cnblogs.com/goloving/p/15393698.html)



## 重写 setItem 事件

```js
const setItem = localStorage.setItem
localStorage.setItem = function (name, value){
setItem.apply(this, arguments)
var event = new Event('setItem')
event.key = name
event.value = value
window.dispatchEvent(event);
}
// 页面使用
window.addEventListener('setItem', function (e) { document.querySelector('.view').innerText = `${e.key}： ${e.value}`
})
```

