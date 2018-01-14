# iFlow简介
iFlow是一个简洁和强大的状态管理库。

[![Travis](https://img.shields.io/travis/unadlib/iflow.svg)](https://travis-ci.org/unadlib/iflow)
[![Coverage Status](https://coveralls.io/repos/github/unadlib/iflow/badge.svg?branch=master)](https://coveralls.io/github/unadlib/iflow?branch=master)
[![npm](https://img.shields.io/npm/v/iflow.svg)](https://www.npmjs.com/package/iflow)
[![Join the chat at https://gitter.im/unadlib/iflow](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/unadlib/iflow?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

### 特性
* **🎯支持普通function和class** - 简洁，自由同时可设计符合各种需求状态管理架构。
* **🏬Store组合** - Store Tree可以很容易共享操作Store节点。
* **⚡动态和热插拔** - 可自由临时插拔State和Action。
* **💥支持异步function以及其他类型的function** - 可任意组合Action或由内部其他内部Action相互组合。
* **🚀强大的中间件** - 中间件可以处理State几乎任何事件。

它是动态的和可扩展的, 你可以直接使用它来添加、删除和修改State和Action。它是 ** Mutable ** 结构, 支持普通function和class, 并且易于面向对象编程。如果使用React, 则需要使用 [react-iflow](https://github/unadlib/react-iflow)连接器。

接下来我们会介绍以下几个方面的情况，让你对iFlow有深入的了解。

* [核心概念与原则](/docs/introduction/ConceptsPrinciples.md)
* [优点](/docs/introduction/Benefits.md)
* [例子: Counter](/docs/introduction/Examples.md) 