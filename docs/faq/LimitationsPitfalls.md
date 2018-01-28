# 限制与陷阱

## 缺陷

* [无调度器在内部自动批处理更新](https://github.com/unadlib/iflow/issues/3)
```javascript
const store = iFlow({
  count(){
    this.counter++
    this.counter++
    this.counter++
  },
  counter: 0,
})
```

例如，这个iFlow store如果执行action `count`,那么iFlow将主动通知三次`counter`更新，虽然同步的更新将被React的更新调度合并成一次，也就是说对于一个普通的同步action内，同一个state同时被改变多次的合并更新调度问题，当前版本iFlow它被忽略了，我们将新增调度器进行修复该问题。

* [未支持Proxy/Reflect的polyfill](https://github.com/unadlib/iflow/issues/2)

由于IE11未支持ES6的Proxy/Reflect，我们将考虑加入Proxy/Reflect的polyfill以便支持IE11。

* [未支持原生无法Proxy的原生类型的原型链函数注入，引发这些类型的改变行为无法自动触发通知](https://github.com/unadlib/iflow/issues/4)

目前已知的不支持类型有：`Set` / `WeakSet` / `Map` / `WeakMap`，很快我们将支持它。

## 陷阱

* 尽可能直接使用引用和拷贝

```javascript
const store = this.props.store
const {foo, bar} = store
```
iFlow不推荐向以上这样例子，state引用关系，我们将监听`store`的全部变化来更新，如果我们需要直接监听更新`foo`和`bar`，那么我们建议直接使用它们的引用或者拷贝。
```javascript
const {foo, bar} = this.props.store
//or
const foo = this.props.store.foo
const bar = this.props.store.bar
```
这样才能仅监听`foo`和`bar`变化更新。

* 在使用immutable时，单层的immutable store才有效

`@immutable`是单层遍历props, 因此iFlow store与普通的对象的混合结构是无效的。
例如
```javascript
class Parent extends Component {
  // this.props.sub is iflow store
  render() {
    return <Sub store={this.props.sub} />
  }
}

@immutable
class Sub extends Component {
  // omit
}
```
这样是有效的。但是下面这个例子是无效的

```javascript
class Parent extends Component {
  // this.props.sub is iflow store
  render() {
    return <Sub store={{foo:'bar', sub: this.props.sub}} />
  }
}

@immutable
class Sub extends Component {
  // omit
}
```
当然，如果你并没有使用`@immutable`你可以任意传递iFlow store

* 关于PureComponent的使用

由于iFlow连接器默认使用mutable store，所以连接器直接与React.PureComponent连接将无法更新，iFlow的连接器对应的组件应该是React.Component，别担心，iFlow将自动进行diff比较，它比React.PureComponent的浅对比更加高效自动。

如果真的需要使用React.PureComponent，那么建议你配合`@immutable`使用。这在子组件性能优化有很大的帮助。

例如
```javascript
@flow(store)
@immutable
class Body extends PureComponent {
  render () {
    return (
      <div>
        <button onClick={() => this.props.store.calculate(-1)}>-</button>
        {this.props.store.counter}
        <button onClick={() => this.props.store.calculate(1)}>+</button>
      </div>
    )
  }
}
```
