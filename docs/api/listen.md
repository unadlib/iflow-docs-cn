# `listen` 方法

### 描述
`listen`用于快速监听在当前Pipe节点下的对应path的值


### 用法
```javascript
listen(store, [path], (value) => {})
```

### 参数
store(Object/Array): 父级store
path(String/Array): 需要取值的path
callback(value(*)): 监听回调函数传递已变化的值

### 返回值
(*): 返回当前Pipe

### 示例
```javascript
import iFlow,{ listen } from 'iflow'
const pipe = iFlow({
  counter: 0,
  foo: {
    bar: 88
  },
}).listen('counter', (counter) => {
  console.log(counter) // log: 1
})
const store = pipe.create()
store.counter = 1
listen(store, ['foo', 'bar'], (foobar) => {
  console.log(foobar) // value: 99
})
store.foo.bar = 99
```
