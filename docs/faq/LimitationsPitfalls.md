# 限制与陷阱

* [无调度器在内部自动批处理更新](https://github.com/unadlib/iflow/issues/3)

对于一个普通的同步流程的action内，同个state被改变多次的合并问题之前被忽略了，我们将进行修复。

* [未支持Proxy/Reflect的polyfill](https://github.com/unadlib/iflow/issues/2)

由于IE11未支持ES6的Proxy/Reflect，我们将考虑加入Proxy/Reflect的polyfill以便支持IE11。

* [未支持原生无法Proxy的原生类型的原型链函数注入，引发这些类型的改变行为无法自动触发通知](https://github.com/unadlib/iflow/issues/4)

目前已知的不支持类型有：`Set` / `WeakSet` / `Map` / `WeakMap`，很快我们将支持它。


