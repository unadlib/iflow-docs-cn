# Cross Store

我们提倡利用iFlow来组合Store，形成一个Store Tree，比较理想的结果就是Store每个节点只能引用和调用它的子节点State或者Actions，但是如果你有Cross Store的需求，那么我们推荐的做法就是每次Cross Store进行引用和调用 另外一个Store的时候，Action可以直接支持，但如果是引用型的State，应该要进行Pipe实例化。

例如：

```javascript
const pipe1 = iFlow({
  foo: {
    bar: ['test']
  }
})
const pipe2 = iFlow({
  foo: {
    bar: iFlow(pipe1.foo.bar)
  }
})
```
⚠️需要特别注意的是: **如果没有这样 pipe 实例化，那么pipe2所引用的pipe1.foo.bar将无法进入中间件系统正常工作。**