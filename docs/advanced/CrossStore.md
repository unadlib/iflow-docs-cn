# 跨节点 Store

我们提倡利用iFlow来组合Store，形成一个Store Tree，比较理想的结果就是Store每个节点只能引用和调用它的子节点State或者Actions，但是如果你有Cross Store的需求，那么我们推荐的做法就是每次Cross Store进行引用和调用另外一个Store的时候，仅支持跨Store调用Action，如果需要跨Store引用State，那么必须Store进行组合。

例如：

```javascript
const store1 = iFlow({
  foo: {
    bar: ['test']
  },
  actionFoo(){
    //
  }
}).create()

const store2 = iFlow({
  foo: {
    bar: store1
  },
  test(){
    store1.actionFoo()
  }
}).create()
```

⚠️需要特别注意的是: **iFlow并不支持pipe组合**
