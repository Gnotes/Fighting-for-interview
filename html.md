# HTML

<details>
<summary>文档流</summary>

> 文档流，指的是元素排版布局过程中，元素会自动从左往右，从上往下的流式排列。并最终窗体自上而下分成一行行, 并在每行中按从左至右的顺序排放元素。脱离文档流即是元素打乱了这个排列，或是从排版中拿走

脱离文档流的方式有两种：**浮动和定位**

#### 参考

- [html/css基础篇——DOM中关于脱离文档流的几种情况分析](https://www.cnblogs.com/chuaWeb/p/html_css_position_float.html)

</details>

<details>
<summary>iframe存在哪些问题</summary>

- 移动端兼容问题不友好。
- 会出现滚动条， 体验不友好。
- 会产生很多页面，开发维护者不容易管理。
- iframe 会阻塞主页面的 onload 事件，导致加载等待漫长，闪屏，体验糟糕。
- 搜索引擎的爬虫无法解读这种页面，不利于 SEO。
- iframe 和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载, 不适合大型网站。

#### 参考

- [iframe 的问题](https://github.com/zanjs/awesome-frontend-interview/issues/9)

</details>

<details>
<summary>BFC (Block Formatting Context)</summary>

> BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，同样的，外面的元素也不会影响到容器里面的子元素

个人理解为 HTML布局、css样式的作用域隔离，作用域内的元素不受外部影响，外部也不会影响到内部

#### 参考

- [学习 BFC (Block Formatting Context)](https://juejin.im/post/59b73d5bf265da064618731d)
- [什么是BFC？对bfc的简单理解](http://www.php.cn/div-tutorial-371936.html)

</details>

<details>
<summary>Meta</summary>

#### 参考

- [Mobile Web Favorites](https://github.com/hoosin/mobile-web-favorites#%E4%BB%8Emeta%E5%BC%80%E5%A7%8B)

</details>

<details>
<summary> img 的title和alt有什么区别</summary>

#### 参考

- [img 的title和alt有什么区别](https://github.com/HerbertKarajan/Fe-Interview-questions/tree/master/23-FE-interview-master)

</details>

<details>
<summary>doctype</summary>

#### 参考

- [doctype](https://github.com/HerbertKarajan/Fe-Interview-questions/tree/master/23-FE-interview-master)

</details>

<details>
<summary>什么是web语义化,有什么好处</summary>

> web语义化是指通过HTML标记表示页面包含的信息，包含了HTML标签的语义化和css命名的语义化。 HTML标签的语义化是指：通过使用包含语义的标签（如h1-h6）恰当地表示文档结构 css命名的语义化是指：为html标签添加有意义的class，id补充未表达的语义

- 去掉样式后页面呈现清晰的结构
- 盲人使用读屏器更好地阅读
- 搜索引擎更好地理解页面，有利于收录
- 便团队项目的可持续运作及维护

#### 参考

- [什么是web语义化,有什么好处](https://github.com/HerbertKarajan/Fe-Interview-questions/tree/master/23-FE-interview-master)

</details>