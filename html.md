# HTML

<details>
<summary>文档流</summary>

> 文档流，指的是元素排版布局过程中，元素会自动从左往右，从上往下的流式排列。并最终窗体自上而下分成一行行, 并在每行中按从左至右的顺序排放元素。脱离文档流即是元素打乱了这个排列，或是从排版中拿走

脱离文档流的方式有两种：**浮动和定位**

对于position: absolute，元素定位将相对于 `最近` 的一个 `relative`、`fixed` 或 `absolute` 的父元素，如果没有则相对于 `body`

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

```html
<!DOCTYPE html>  H5标准声明，使用 HTML5 doctype，不区分大小写
<head lang=”en”> 标准的 lang 属性写法
<meta charset=’utf-8′>    声明文档使用的字符编码
<meta http-equiv=”X-UA-Compatible” content=”IE=edge,chrome=1″/>   优先使用 IE 最新版本和 Chrome
<meta name=”description” content=”不超过150个字符”/>       页面描述
<meta name=”keywords” content=””/>      页面关键词
<meta name=”author” content=”name, email@gmail.com”/>    网页作者
<meta name=”robots” content=”index,follow”/>      搜索引擎抓取
<meta name=”viewport” content=”initial-scale=1, maximum-scale=3, minimum-scale=1, user-scalable=no”> 为移动设备添加 viewport
<meta name=”apple-mobile-web-app-title” content=”标题”> iOS 设备 begin
<meta name=”apple-mobile-web-app-capable” content=”yes”/>  添加到主屏后的标题（iOS 6 新增）
是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏
<meta name=”apple-itunes-app” content=”app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL”>
添加智能 App 广告条 Smart App Banner（iOS 6+ Safari）
<meta name=”apple-mobile-web-app-status-bar-style” content=”black”/>
<meta name=”format-detection” content=”telphone=no, email=no”/>  设置苹果工具栏颜色
<meta name=”renderer” content=”webkit”>  启用360浏览器的极速模式(webkit)
<meta http-equiv=”X-UA-Compatible” content=”IE=edge”>     避免IE使用兼容模式
<meta http-equiv=”Cache-Control” content=”no-siteapp” />    不让百度转码
<meta name=”HandheldFriendly” content=”true”>     针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓
<meta name=”MobileOptimized” content=”320″>   微软的老式浏览器
<meta name=”screen-orientation” content=”portrait”>   uc强制竖屏
<meta name=”x5-orientation” content=”portrait”>    QQ强制竖屏
<meta name=”full-screen” content=”yes”>              UC强制全屏
<meta name=”x5-fullscreen” content=”true”>       QQ强制全屏
<meta name=”browsermode” content=”application”>   UC应用模式
<meta name=”x5-page-mode” content=”app”>    QQ应用模式
<meta name=”msapplication-tap-highlight” content=”no”>    windows phone 点击无高光
设置页面不缓存
<meta http-equiv=”pragma” content=”no-cache”>
<meta http-equiv=”cache-control” content=”no-cache”>
<meta http-equiv=”expires” content=”0″>
```

#### 参考

- [Mobile Web Favorites](https://github.com/hoosin/mobile-web-favorites#%E4%BB%8Emeta%E5%BC%80%E5%A7%8B)
- [前端常见面试题汇总](https://www.geekjc.com/ebook/detail/5ba5bcae7143880b09cb4d54)

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

<details>
<summary>div+css的布局较table布局有什么优点</summary>

- 改版的时候更方便 只要改css文件。
- 页面加载速度更快、结构化清晰、页面显示简洁。
- 表现与结构相分离。
- 易于优化（seo）搜索引擎更友好，排名更容易靠前。

#### 参考

- [div+css的布局较table布局有什么优点](http://www.cnblogs.com/coco1s/p/4034937.html)

</details>

<details>
<summary>微格式</summary>

> 微格式（Microformats）是一种让机器可读的语义化XHTML词汇的集合，是结构化数据的开放标准。是为特殊应用而制定的特殊格式。

- 优点：将智能数据添加到网页上，让网站内容在搜索引擎结果界面可以显示额外的提示

#### 参考

- [微格式](http://www.cnblogs.com/coco1s/p/4034937.html)

</details>

<details>
<summary>什么是外边距重叠？重叠的结果是什么？</summary>

外边距重叠就是margin-collapse。

> 在CSS当中，相邻的两个盒子（可能是兄弟关系也可能是祖先关系）的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。

**折叠结果遵循下列计算规则：**

- 两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
- 两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
- 两个外边距一正一负时，折叠结果是两者的相加的和。

#### 参考

- [什么是外边距重叠？重叠的结果是什么？](http://www.cnblogs.com/coco1s/p/4034937.html)

</details>

<details>
<summary>HTML中常用标签中英文？</summary>

- ul ：是unordered lists的缩写 (无序列表)
- li ：是list item的缩写 （列表项目）
- ol ：是ordered lists的缩写（有序列表）
- dl ：是definition lists的英文缩写 (自定义列表)
- dt ：是definition term的缩写 (自定义列表组)
- dd ：是definition description的缩写（自定义列表描述）
- nl ：是navigation lists的英文缩写 （导航列表）
- tr ：是table row的缩写 （表格中的一行）
- th ：是table header cell的缩写 （表格中的表头）
- td ：是table data cell的缩写 （表格中的一个单元格）
- cell ：单元格
- cellpadding ：（单元格补白）
- cellspacing ：（单元格边距）

#### 参考

- [网页设计中常用标签英文？](https://www.zhihu.com/question/48053930)

</details>

<details>
<summary>WebGL</summary>

> WebGL (Web图形库) 是一种JavaScript API，用于在任何兼容的Web浏览器中呈现交互式3D和2D图形，而无需使用插件。WebGL通过引入一个与OpenGL ES 2.0紧密相符合的API，可以在HTML5 <canvas> 元素中使用

#### 参考

- [WebGL](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API)

</details>

<details>
<summary>HTML 中的7阶层叠水平（元素的层叠关系）</summary>

> `正 z-index` > `inline-block` > `float` > `block` > `负 z-index` > `backgroud`

也就是说： 除了`z-index`外，还有这些层级的关系

#### 参考

- [关于元素层级的一些介绍](http://www.cnblogs.com/zourong/p/5465953.html)

</details>

<details>
<summary>DOM 事件的深入浅出</summary>

#### 参考

- [DOM 事件的深入浅出](https://www.jb51.net/article/99094.htm)
- [DOM 事件的深入浅出](https://www.jb51.net/article/99099.htm)

</details>