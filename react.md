# React & React Native

<details>
<summary>为什么循环产生的组件中要利用上key这个特殊的prop？</summary>

Keys负责帮助React跟踪列表中哪些元素被改变/添加/移除。React利用子元素的key在比较两棵树的时候，快速得知一个元素是新的还是刚刚被移除。没有keys，React也就不知道当前哪一个的item被移除了

#### 参考

- [Questions-and-Answers](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers)

</details>

<details>
<summary>React-router 路由的实现原理?</summary>

#### 参考

- [react-router的实现原理](https://blog.csdn.net/tangzhl/article/details/79696055)
- [React-router路由基本原理](https://blog.csdn.net/leviscar/article/details/81878677)

</details>

<details>
<summary>说说React Native 框架的实现原理？</summary>

- React Native如何把React转化为原生API

> React Native会在一开始生成OC模块表，然后把这个模块表传入JS中，JS参照模块表，就能间接调用OC的代码。
相当于买了一个机器人（OC），对应一份说明书（模块表），用户（JS）参照说明书去执行机器人的操作

- React Native是如何做到JS和OC交互

> iOS原生API有个JavaScriptCore框架，通过它就能实现JS和OC交互，想了解JavaScriptCore，请点击JavaScriptCore
1.首先写好JSX代码（React框架就是使用JSX语法）
2.把JSX代码解析成javaScript代码
3.OC读取JS文件
4.把javaScript代码读取出来，利用JavaScriptCore执行
5.javaScript代码返回一个数组，数组中会描述OC对象，OC对象的属性，OC对象所需要执行的方法，这样就能让这个对象设置属性，并且调用方法

#### 参考

- [【React Native】从源码一步一步解析它的实现原理](https://www.jianshu.com/p/5cc61ec04b39)

</details>

<details>
<summary>受控组件(Controlled Component)与非受控组件(Uncontrolled Component)的区别</summary>

#### 参考

- [
React中受控与非受控组件](https://segmentfault.com/a/1190000012404114)

</details>

<details>
<summary>React.Component与React.PureComponent的区别？</summary>

> `React.PureComponent` 与 `React.Component` 几乎完全相同，但 `React.PureComponent` 通过 prop 和 state 的 **`浅对比`** 来实现 shouldComponentUpate()

> React.PureComponent 的 shouldComponentUpdate() 只会对对象进行浅对比。如果对象包含复杂的数据结构，它可能会因深层的数据不一致而产生错误的否定判断(表现为对象深层的数据已改变视图却没有更新, 原文：false-negatives)。当你期望只拥有简单的props和state时，才去继承 PureComponent ，或者在你知道深层的数据结构已经发生改变时使用 forceUpate() 。或者，考虑使用 不可变对象 来促进嵌套数据的快速比较。  

> 此外,React.PureComponent 的 shouldComponentUpate() 会忽略整个组件的子级。请确保所有的子级组件也是”Pure”的

#### 参考

- [React.PureComponent](https://react.docschina.org/docs/react-api.html#reactpurecomponent)

</details>

<details>
<summary>为什么在需要在没有使用React的jsx模块中需要引入 import React from 'react' ？</summary>

> 每个JSX元素仅是调用 `React.createElement` 的语法糖，因此在调用 `render` 方式时，底层实现其实依然调用的是 `React.createElement`，因此需要引入

#### 参考

- [Creating React Elements](https://react.docschina.org/docs/react-api.html#creating-react-elements)

</details>

<details>
<summary>React 源码剖析</summary>

- [React源码解析](https://juejin.im/post/5a84682ef265da4e83266cc4)
- [React 源码全方位剖析](http://www.sosout.com/2018/08/12/react-source-analysis.html)

</details>