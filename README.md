<a href='http://cn.iflow.js.org'><img src='https://raw.githubusercontent.com/unadlib/iflow/master/assets/logo.png' height='60' alt='iFlow Logo' aria-label='cn.iflow.js.org' /></a>

iFlow是一个简洁和强大的状态管理库，iFlow没有任何依赖包，且非常小(5k)。

[![Travis](https://img.shields.io/travis/unadlib/iflow.svg)](https://travis-ci.org/unadlib/iflow)
[![Coverage Status](https://coveralls.io/repos/github/unadlib/iflow/badge.svg?branch=master)](https://coveralls.io/github/unadlib/iflow?branch=master)
[![npm](https://img.shields.io/npm/v/iflow.svg)](https://www.npmjs.com/package/iflow)
[![Join the chat at https://gitter.im/unadlib/iflow](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/unadlib/iflow?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

>[从0开始5分钟完成TODO](http://cn.iflow.js.org/#%E4%BB%8E0%E5%BC%80%E5%A7%8B%E4%BA%94%E5%88%86%E9%92%9F%E5%AE%8C%E6%88%90todo)

### 特性
* **🎯支持普通function和class** - 简洁，自由同时可设计符合各种需求状态管理架构。
* **🏬Store组合** - Store Tree可以很容易共享操作Store节点。
* **⚡动态和热插拔** - 可自由临时插拔State和Action。
* **💥支持异步function以及其他类型的function** - 可任意组合Action或由内部其他内部Action相互组合。
* **🚀强大的中间件** - 中间件可以处理State几乎任何事件。
* **🔥Store支持immutable** - Store是支持被处理成immutable的Store。

它是动态的和可扩展的, 你可以直接使用它来添加、删除和修改State和Action。它是 **Mutable** 结构, 支持普通function和class, 并且易于面向对象编程。如果使用React, 则需要使用 [react-iflow](https://github/unadlib/react-iflow)连接器。

接下来我们会介绍以下几个方面的情况，让你对iFlow有深入的了解。

* [概念与原则](/docs/introduction/ConceptsPrinciples.md)
* [优点](/docs/introduction/Benefits.md)
* [例子: Counter](/docs/introduction/Examples.md) 

### 从0开始五分钟完成TODO

1.首先我们先完成快速一个TODO项目配置和基本npm包依赖

```bash
mkdir example && cd example
yarn init -y
yarn add -D parcel-bundler babel-cli babel-preset-react babel-preset-env
yarn add react react-dom iflow react-iflow
```

2.然后我们完成一个babel配置文件和App入口文件index.html

```bash
echo '{"presets": ["env","react"]}' > .babelrc
echo '<div id="app"></div><script src="./index.js"></script>' > index.html
```

3.接着我们完成一个简单的TODO

```bash
cat <<EOF > index.js
import React from 'react'
import ReactDOM from 'react-dom'
import iFlow from 'iflow'
import flow from 'react-iflow'

const store = iFlow({
    todo: [],
    add(text){this.todo.push({text})},
    toggle(item){item.completed = !item.completed}
}).create()

const App = flow(store)(class extends React.Component {
    render() {
        const {todo,add,toggle} = this.props.store
        return (
            <div>
                <input ref={(ref)=>this.input=ref}/>
                <button onClick={()=>{add(this.input.value);this.input.value=''}}>Add</button>
                <ul>
                    {todo.map((item,key)=>(<li key={key} style={item.completed?{textDecoration:'line-through'}:{}} onClick={()=>toggle(item)}>{item.text}</li>))}
                </ul> 
            </div>
        )
    }
})

ReactDOM.render(<App/>,document.getElementById('app'))
EOF
```

4.最后我们运行起来, 嘿嘿!🎉🎉🎉

```bash
npx parcel index.html
```

