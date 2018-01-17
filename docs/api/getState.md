# `getState` 方法

### 描述
`getState`用于Pipe得到当前的store的状态树


### 用法
```javascript
getState(store)
```

### 参数
store(Array/Object): 当前store

### 返回值
(Array/Object): 返回需要取值的path的状态

### 示例
```javascript
import iFlow,{ getState } from 'iflow'
const pipe = iFlow({
  calculate: external(async function (number) {
    // do async something
  }),
  counter: 0,
  foo: {
    bar: 88
  },
})
const store = pipe.create()
getState(store) // value: { counter: 0, foo: { bar: 88 } }
```
