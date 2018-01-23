# Immutable

Immutable能带来很多好处，但是如果利用解构操作或者是Object.assign，事实上并不利于开发者维护相关代码，或者直接引入Immutable.js会带的转换消耗的弊端。

虽然iFlow内部实现一个完整的immutable，但也可以利于iFlow的`getState`内部API结合`addObserver`中间件来实现自定义的immutable, iFlow提供[getImmutable](/docs/api/getImmutable.md)API。

⚠️需要注意的是：**iFlow提供的getImmutable API得到的store是没有Object.freeze的immutable**