# 一个简洁且强大的状态管理库 - iFlow

### 前言

> 以React为主的开发过程中，用过主流的两大状态管理工具Redux和Mobx。但在我使用它们时，逐渐地感觉到了一些不太好的地方：Redux使用过程有点冗余和拖沓，而尽管Redux也有中间件，但Redux带来的收益和它官方说的一样：仅仅只是一个纯的状态容器而不是状态管理；而基于Observable的状态管理库Mobx则侵入性强，且丢失状态类型的原始性(被Observable实例化)，以及因此而导致一系列限制与困扰。

所以，我慢慢开始期待有一个更好的状态管理库出现。我希望它基于Mutable结构，状态操作高效直接，而且不应破坏状态数据类型的原始性；同时它也支持Immutable输出，兼顾Mutable的有利于编程和操作；然后它应该是渐进式的，可以是简洁的，也能是强大的，不会因过多的繁琐冗余操作以及复杂概念而破坏编程乐趣。

是的，它应该是简洁且强大的。

因此，我试着构建一个这样状态管库库 -- [iFlow](https://github.com/unadlib/iflow)。

### 简介

它是动态的和可扩展的, 你可以直接使用它来添加、删除和修改State和Action；它是Mutable结构, 支持普通function和class, 并易于面向对象编程；同时它没有任何依赖包，且非常小(5k)。需要特别说明的是iFlow定义的Store是包括Actions和State。如果你是刚涉猎状态库，那么在快速阅读文档后就可以让你在几分钟内就能开始构建你的App了，因为它是极简单的；但如果你谙熟各种状态库，那么它也能让你得心应手，或许你会惊喜的发现，iFlow有些特性能高效实现出其他状态库不太好实现的架构设计。

从原理上说，iFlow是基于ECMAScript 2015的Proxy & Reflect。它构建的Paths Match大致过程是：先在View组件进行引用Store的State的时候获得Getter Paths；然后在Action被触发的时候，将通过Proxy得到Setter Path；最后，在通过观察者来传递Setter Path，并在连接器内进行快速地Getter Paths Match来控制更新View组件。

* 流程图

![流程图](https://raw.githubusercontent.com/unadlib/iflow/master/assets/flowChart.png)

> 抽象化公式来表达就是: action(store) => store = newStore

### 基本概念

* Store

它包含State和Actions，是State管理应用的具体部分，它可以是普通对象的形式，也可以是class的形式。

```javascript
const store = iFlow({
  add (number) {
    this.counter += number
  },
  counter: 0,
}).create()
```
或者是
```javascript
class Counter {
  constructor () {
    this.counter = 0
  }
  add (number) {
    this.counter += number
  }
}
const store = iFlow(new Counter()).create()
```

* 中间件(Middleware)

iFlow提供了好几种不同类型的中间件用于控制不同流程下的Action运行和State改变，它能控制几乎State和Action的一切行为。

```javascript
const store = iFlow({
  add (number) {
    this.counter += number
  },
  counter: 0,
}).middleware({
  stateDidChange (...args) {
    // 状态变化后通知中间件
  }
}).create()
```

* 连接器(Connector)

iFlow配合react-iflow连接器，可以让Store和View层连接，iflow的连接器是简单高效的。

```javascript
class Example extends Component {
  render () {
    return <div onClick={() => this.props.store.add(1)}>{this.props.store.counter}</div>
  }
}
export default flow(store)(Example)
```

### 特性
* 🎯支持普通function和class - 它很简单，同时也可设计符合各种需求状态管理架构。
* 🏬Store组合 - Store Tree可以很容易共享操作Store节点。
* ⚡动态和热插拔 - 可自由插拔State和Action。
* 💥支持异步function以及其他类型的function - 可任意组合Action或由内部其他内部Action相互组合。
* 🚀强大的中间件 - 中间件可以拦截控制和处理全部的State变化和Actions运行。
* 🔥Store支持immutable - Store是支持被处理成immutable的Store。

### 优点

* 保持数据结构的原始性

iFlow因为利用ES6 Proxy机制，因此可以保持数据结构的完整原始性，同时支持异步函数以及其他类型函数，当然也包括普通的类和函数。

* 无样板代码

iFlow能给你比较自由的使用它来实现属于符合实际开发需求的状态数据结构设计，而不会因为各种库的限制来变成有过多的样板代码。

* 易于面向对象

有时当我们需要解耦业务代码，尤其是中大型的项目中，我们可能需要一些面向对象编程当设计，所以状态库如果能支持它当然是更好了。

* 尽可能少的选择器

在使用Web框架(如React)的时候，iFlow是自动匹配更新的，与之对应的连接库react-iflow可以让你能尽可能少一些选择器的编写与操作，甚至在大多数情况下，使用iFlow时，你不需要选择器。

* 强大的中间件

如果有必要，事实上iFlow中间件是强大且有用的，完整的中间件：stateWillInitialize / actionWillStart / stateWillChange / stateDidChange / actionDidEnd，它们能实现各种需求下的状态管理设计； 同时它也可以实现一些基础性中间件，例如持久化中间件等等.

* 可组合和可伸缩的Store

iFlow提倡将Store组合成store tree,不用担心无关的Store造成一些性能上的影响，因为它是动态匹配的，你可以放心自由的组合和拓展Store。

### 示例

* 简单计数器

我们来构建一个简单的计数器：

```javascript
const store = iFlow({
  calculate (number) {
    this.counter += number
  },
  counter: 0,
}).create()

@flow(store)
class Counter extends Component {
  render() {
    return (
        <div>
        <button onClick={() => this.props.store.calculate(-1)}>-</button>
        {this.props.store.counter}
        <button onClick={() => this.props.store.calculate(1)}>+</button>
      </div>
    );
  }
}
```

[在线运行](https://jsfiddle.net/unadlib/03ukqj5L/)

* TODO

接着我们来实现一个复杂点的[TODO](https://github.com/unadlib/iflow/tree/master/examples/todo)(带撤销/重做/持久化)

[在线运行](https://jsfiddle.net/unadlib/6wabhdqp/)

### 尾声

最后，iFlow希望能为开发者解决在状态管理架构和设计上可能需要的困扰，小型项目能够更简单轻量化，大型项目又能够高效地各种深度设计。

---

如果你对iFlow感兴趣的话，非常欢迎来尝试看看，同时也非常欢迎提交PR和issue。

目前iFlow部分文档已提供，同时后续文档也将继续完善中。

如果感觉它还不错，特别欢迎给[iFlow](https://github.com/unadlib/iflow)一个star，谢谢鼓励！！！

[https://github.com/unadlib/iflow](https://github.com/unadlib/iflow)
