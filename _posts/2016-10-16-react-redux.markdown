---
layout:     post
title:      "React-Redux--理论篇"
subtitle:   " \"单向数据流设计\""
date:       2016-10-16 12:00:00
author:     "Xz Yao"
header-img: "img/post-bg-react-redux.jpg"
catalog: true
tags:
    - Technology
    - Engineering
---

### 何为Redux

React是一种建立用户界面的框架，在React中，每一个UI元素被认为是一个“组件”，组件之间嵌套组合，就形成了一个组件树。数据在这个组件树中单向流动，即上层组件可以向下层组件传递数据，而下层组件不能向上层组件传递数据，兄弟组件之间也不能。但我们希望，数据能够自由地流通，这要怎么办呢？有几种方式都可以来实现：

* 上层组件向下层组件传递一个回调函数，下层组件调用回调函数来修改上层组件的值。
* 下层组件发布一个订阅来通知上层组件更新数据。
* Facebook提出了flux的模式，如果说React是MVC中V的部分，那么Flux就可以被认为是M和C的部分。稍后我们也会简单地介绍一下flux。
* 一些其他的方式，如Redux(其实是Flux的简化版)。 

#### 何为Flux

前两种形式在小应用中是比较多见的。而随着项目越来越大，回调函数或是事件会越来越多，会给管理带来很大的麻烦。Flux通过一个更加抽象的单向数据流来解决组件树中数据流动的问题。

![Flux Architecture](https://hulufei.gitbooks.io/react-tutorial/content/image/flux-overview.png)

当有Web请求或是用户的操作时，通过ActionCreator创建一个Action，这个Action由Dispatcher送给Store，之后再通知页面元素进行渲染。

![Flux UDF](https://raw.githubusercontent.com/facebook/flux/master/docs/img/flux-diagram-white-background.png)

* 首先要有 action，通过定义一些 action creator 方法根据需要创建 Action 提供给 dispatcher
* View 层通过用户交互（比如 onClick）会触发 Action
* Dispatcher 会分发触发的 Action 给所有注册的 Store 的回调函数
* Store 回调函数根据接收的 Action 更新自身数据之后会触发一个 change 事件通知 View 数据更改了
* View 会监听这个 change 事件，拿到对应的新数据并调用 setState 更新组件 UI

这样设计之后，所有的状态(state)都是由store来维护，需要时去store中取即可。所有的行为都是在actions中，逻辑和页面可以分割开。这构成了上图中的单向数据循环。

Flux会更像一种模式(Pattern)，而不是一种框架。因此它也没有特别的硬依赖，我们完全可以自己实现一个这样的设计。

#### MVC和Flux

MVC也是Web中常见的一种模式。

在MVC模式中，View和Model之间可以是多对多的关系，也就是说，单个视图上的数据可能来自于多个模型，单个模型的数据更新也可能需要通知多个视图。那么会不会造成死锁呢？

![Complex MVC](http://cdn1.infoqstatic.com/statics_s2_20161011-0321/resource/news/2014/05/facebook-mvc-flux/zh/resources/0519000.png)

(然而实际开发中，是比较少会遇到这种情况的吧？而且也有一些的分区方式)

其实关于MVC，大家的争议也挺多的。

![Simple Flux](http://cdn1.infoqstatic.com/statics_s2_20161011-0321/resource/news/2014/05/facebook-mvc-flux/zh/resources/0519001.png)

Flux的结构图看上去就清晰多了。

但是从我的感觉来看，MVC和Flux其实是有很多相近之处的，

#### Redux is Implement

Redux可以说是Flux思想的一种实现。在Flux中Action、Dispatcher、Store的基础上，Redux又引入了两个概念Reducers和Middleware.

##### Store

Redux中所有的状态都储存在唯一的Store中，它是**数据存储中心**，连接着Action和View。

Store所做的工作主要是：

1. 接收Views传来的Action

Store通过dispatch方法来接收views中传来的action，例如：
```
store.dispatch({
    type: 'INCREMENT'
});
```
2. 根据Action的类型Type和数据Payload，对Store中已有的数据进行修改

Store在Reducers中修改数据。实际上，在Store被创建的时候，就会将一个Reducer传给它，这样Store就可以通过Reducer的返回值来更新数据了。
```
// 一个reducer
function counterReducer(state = 0, action) {
  switch (action.type) {
    case 'INCREASE':
      return state + 1;
    case 'DECREASE':
      return state - 1;
    default:
      return state;
  }
}

// 传递reducer作为形参
let store = Redux.createStore(counterReducer);
```
3. 通知View数据有改变

Store的实例会有一个subscribe方法，Views中只需要调用该方法注册一个回调函数，之后在每次Dispatch之后，该回调都会被触发，从而实现重新渲染。如果需要取到当前的Store数据，也可以通过```getStore()```方法来获得。
```
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);
```

##### Reducer

Reducer是一个函数，输入是旧的状态，输出新的状态。Redux与Flux最大的区别在于Redux中只有一个Store，所有的状态都存储在一个Store中。这是为了避免Store之间存在复杂的嵌套关系带来的弊病。但是，这样的设计会带来另一个副作用，如这个状态树可能变成：
```
let store = {
    a: 1,
    b: {
        c: true,
        d: {
            e: [2, 3]
        }
    }
};
```
 ```store.b.d.e```，这简直是个噩梦。

所以，Redux使用Reducers来对数据进行拆解和访问，我们直接将子树传给reducer，处理之后返回。这样就可以完成对一部分数据的处理。之后，如果我们能够将这些reducers结合起来，形成一个大的reducer，就可以非常方便而又解决了嵌套问题。这就是```combineReducers```。

另一个问题是，在我们渲染界面时，经常需要判断元素是否有变更。对于字符串、数字来说，这样的判断是非常容易的。但是对于对象的判断就比较复杂。
```
let prevProps = {
    e: [2, 3]
};

let nextProps = prevProps;

nextProps.e.push(4);

console.log(prevProps.e === nextProps.e); // 始终为true
```

虽然有方法可以避免，但是依然是一个坑。而在reducer中，我们始终返回的是一个新的引用。这样就可以比较轻松地进行判断了。

```
function eReducer(state = [2, 3], action) {
  switch (action.type) {
    case 'ADD':
      return [...state, 4]; // 并没有直接地通过state.push(4)，修改引用的数据
    default:
      return state;
  }
}
```

这种思想其实是来自于函数式编程。函数式编程的思想是把运算过程写成一系列的函数。例如，我们要求```(1 + 2) * 3 - 4```这样一个变量的值，在函数式编程里，我们会实现```multiply(),add(),subtract()```等几个方法，之后```var result = subtract(multiply(add(1,2), 3), 4);```即可。

函数式编程具有五个特点：
1. 函数是"第一等公民"
2. 只用"表达式"，不用"语句"
3. 没有"副作用"
4. 不修改状态，只返回新的值，不修改变量
5. 引用透明，返回值只与传入的参数有关。不依赖于外部变量

### React-Redux

React-Redux是为了让Redux和React更好地结合的一个库。它主要暴露两个API

1. Provider 组件
Provider将Store中的数据传递给组件
2. Connect 方法
Connect让Component和Store进行关联，即Store的数据变化可以通知组件进行渲染。

