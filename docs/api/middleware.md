# “middleware” **Pipe**方法

### 描述
`middleware`是iFlow中间件组API，和对应是各类型和中间件用法完全等价。

* 中间件对照表如下： 

| 中间件API    | 直接接口API          | return | return value       | 异步  | 说明                       |
| :---------- | :-----------------: | :----: | :----------------: | :---: | ------------------------: | 
| stateWillInitialize        | [setInitializeValue](/docs/api/setInitializeValue.md)  | ✅     | 可添加初始化的值     | ❌     | 初始化中间件                |
| actionWillStart       | [addInterceptor](/docs/api/addInterceptor.md)      | ✅     | 可改变action参数    | ✅     | Action前置中间件             |
| stateWillChange      | [addMiddleware](/docs/api/addMiddleware.md)       | ✅     | 可改变set的值       | ❌     | State Change前置中间件      |
| stateDidChange       | [addObserver](addObserver.md)         | ❌     | -                  | ❌     | State Change后置通知中间件   | 
| actionDidEnd         | [addListener](addListener.md)         | ❌     | -                  | ✅     | Action后置通知中间件         |

### 用法

```javascript
pipe.middleware({
    stateWillInitialize: (...args) => {},
    actionWillStart: (...args) => {},
    stateWillChange: (...args) => {},
    stateDidChange: (...args) => {},
    actionDidEnd: (...args) => {},
})
```

```javascript
pipe.middleware(
  {
    stateWillInitialize: (...args) => {},
    actionWillStart: (...args) => {},
    stateWillChange: (...args) => {},
    stateDidChange: (...args) => {},
    actionDidEnd: (...args) => {},
  },
  {
    stateWillInitialize: (...args) => {},
    actionWillStart: (...args) => {},
    stateWillChange: (...args) => {},
    stateDidChange: (...args) => {},
    actionDidEnd: (...args) => {},
  })
```

```javascript
pipe
.middleware({
    stateWillInitialize: (...args) => {},
    actionWillStart: (...args) => {},
    stateWillChange: (...args) => {},
    stateDidChange: (...args) => {},
    actionDidEnd: (...args) => {},
})
.middleware({
    stateWillInitialize: (...args) => {},
    actionWillStart: (...args) => {},
    stateWillChange: (...args) => {},
    stateDidChange: (...args) => {},
    actionDidEnd: (...args) => {},
})
```

