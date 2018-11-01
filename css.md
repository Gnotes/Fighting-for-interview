# CSS

<details>
<summary>文本超出隐藏</summary>

#### 单行

```css
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
```

#### 多行

- 适用于WebKit浏览器及移动端

```css
display: -webkit-box;
-webkit-line-clamp: 3; 
-webkit-box-orient: vertical;
overflow: hidden;
```

注：

- `-webkit-line-clamp` 用来限制在一个块元素显示的文本的行数。 为了实现该效果，它需要组合其他的WebKit属性。常见结合属性：
- `display: -webkit-box` 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。
- `-webkit-box-orient` 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。

#### 动态计算(结合JS实现)

```
通过生成一个同等宽度 并隐藏的盒模型（div 或者 p） ，然后将文字放入，通过计算行高的方式，递归截取相应长度的文字
```

#### 参考

- [awesome-frontend-interview](https://github.com/zanjs/awesome-frontend-interview/issues/77)

</details>

<details>
<summary>link与@import 的区别</summary>

- 属性功能差别

```
link属于XHTML标签，而@import完全是CSS提供的一种方式。 link标签除了可以加载CSS外，还可以做很多其它的事情，比如定义RSS，定义rel连接属性等，@import就只能加载CSS了
```

- 加载顺序的差别

```
link 会随页面载入，而 @import 引入的 CSS 要等页面加载完，再进行载入。所以有时候浏览@import加载CSS的页面时开始会没有样式（就是闪烁），网速慢的时候还挺明显
```

- 兼容性的差别

```
由于@import是CSS2.1提出的所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题
```

- dom控制样式时的差别

```
当使用javascript控制dom去改变样式的时候，只能使用link标签，因为@import不是dom可以控制的
```

- @import 引入其他样式文件

```
@import可以在css中再次引入其他样式表
```

如：

```css
/* index.css */
@import "other.css";

...

```

```css
/* other.css */
p {color:red;}
```

#### 参考

- [CSS加载方式link和@import的区别](https://blog.csdn.net/hangxingkong/article/details/51645971)

</details>

<details>
<summary>CSS 水平垂直居中</summary>

#### 仅居中元素宽高确定时适用

```html
<div class="outter">
  <div class="inner"></div>
</div>
```

- absolute + 负margin

```css
.outter {
  width: 300px;
  height: 300px;
}

.innner {
  width: 100px;
  height: 100px;

  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -50px;
  margin-top: -50px;
```

- absolute + margin auto

```css
.outter {
  width: 300px;
  height: 300px;

  position: relative;
}

.innner {
  width: 100px;
  height: 100px;

  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
```

- absolute + calc

```css
.outter {
  width: 300px;
  height: 300px;

  position: relative;
}

.innner {
  width: 100px;
  height: 100px;

  position: absolute;
  top: calc(50% - 50px);
  left: calc(50% - 50px);
```

#### 居中元素不定宽高适用

- absolute + transform

```css
.outter {
  width: 300px;
  height: 300px;

  position: relative;
}

.innner {
  width: 100px;
  height: 100px;

  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

- [writing-mode](https://www.cnblogs.com/xiaofenguo/p/6168865.html)

- lineheight

```css
.outter {
  line-height: 300px;
  text-align: center;
  font-size: 0px;
}

.innner {
  font-size: 16px;
  display: inline-block;
  vertical-align: middle;
  line-height: initial;
```

- table-cell

```css
.outter {
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}

.innner {
  display: inline-block;
}
```

- flex

```css
.outter {
  display: flex;
  justify-content: center;
  align-items: center;
}

.innner {
  display: inline-block;
}
```

- grid

```css
.outter {
  display: grid;
}

.innner {
  align-self: center;
  justify-self: center;
}
```


#### 参考

- [水平垂直居中](https://github.com/yanhaijing/vertical-center)

</details>

<details>
<summary>移动端 1px 被 渲染成 2px 问题</summary>

> 开发移动端web项目时经常遇到设置border:1px，但是显示的边框却为2px或是3px粗细，这是因为设备像素比devicePixelRatio为2或3引起的

#### 参考

- [设备像素比devicePixelRatio简单介绍](https://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/)
- [7种方法解决移动端Retina屏幕1px边框问题](https://www.jianshu.com/p/7e63f5a32636)

</details>

<details>
<summary>css 清除浮动</summary>

#### 参考

- [css 清除浮动](https://www.cnblogs.com/Gabriel-Wei/p/6184392.html)

</details>

<details>
<summary>手机布局适配</summary>

#### 参考

- [了解真实的『REM』手机屏幕适配](https://github.com/hbxeagle/rem/blob/master/README.md)
- [前端：『REM』手机屏幕高清适配方案](https://github.com/hbxeagle/rem/blob/master/HD_ADAPTER.md)

</details>

<details>
<summary>css reset</summary>

> HTML标签在浏览器中都有默认的样式，不同的浏览器的默认样式之间存在差别,默认样式可能会给我们带来多浏览器兼容性问题，影响开发效率

#### 参考

- [css-reset 代码](https://segmentfault.com/a/1190000009369872)

</details>

<details>
<summary>css选择器</summary>

- 标签选择
- 类选择器
- ID选择器
- 全局选择器
- 组合选择器
- 继承选择器
- 伪类选择器
- 字符串匹配的属性选择符

#### 参考

- [CSS 选择器参考手册](http://www.w3school.com.cn/cssref/css_selectors.asp)

</details>

<details>
<summary>css 优先级（权重）</summary>

`!important(不可更改)` > `内联样式(1000)` > `ID选择器(100)` > `类选择器(10)` > `标签选择器(1)`

#### 参考

- [css 样式重写无效， 如何生效 !important](https://github.com/zanjs/awesome-frontend-interview/issues/38)

</details>

<details>
<summary>CSS Hack</summary>

> 不同的浏览器对CSS的解析结果是不同的，因此会导致相同的CSS输出的页面效果不同，这就需要CSS Hack来解决浏览器局部的兼容性问题。而这个针对不同的浏览器写不同的CSS 代码的过程，就叫CSS Hack

#### 常用的 CSS Hack

```css
/* CSS属性级Hack */
color:red; /* 所有浏览器可识别*/
_color:red; /* 仅IE6 识别 */
*color:red; /* IE6、IE7 识别 */
+color:red; /* IE6、IE7 识别 */
*+color:red; /* IE6、IE7 识别 */
[color:red; /* IE6、IE7 识别 */
color:red9; /* IE6、IE7、IE8、IE9 识别 */
color:red; /* IE8、IE9 识别*/
color:red9; /* 仅IE9识别 */
color:red ; /* 仅IE9识别 */
color:red!important; /* IE6 不识别!important*/
```

```css
/* CSS选择符级Hack */
*html #demo { color:red;} /* 仅IE6 识别 */
*+html #demo { color:red;} /* 仅IE7 识别 */
body:nth-of-type(1) #demo { color:red;} /* IE9+、FF3.5+、Chrome、Safari、Opera 可以识别 */
head:first-child+body #demo { color:red; } /* IE7+、FF、Chrome、Safari、Opera 可以识别 */
:root #demo { color:red9; } : /* 仅IE9识别 */
```

```css
/* IE条件注释Hack */
<!--[if IE]>此处内容只有IE可见<![endif]--> 
<!--[if IE 6]>此处内容只有IE6.0可见<![endif]--> 
<!--[if IE 7]>此处内容只有IE7.0可见<![endif]--> 
<!--[if !IE 7]>此处内容只有IE7不能识别，其他版本都能识别，当然要在IE5以上。<![endif]-->
<!--[if gt IE 6]> IE6以上版本可识别,IE6无法识别 <![endif]-->
<!--[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]-->
<!--[if lt IE 7]> 低于IE7的版本才能识别，IE7无法识别。 <![endif]-->
<!--[if lte IE 7]> IE7以及IE7以下版本可识别<![endif]-->
<!--[if !IE]>此处内容只有非IE可见<![endif]-->
```

#### 参考

- [CSS Hack](https://github.com/zanjs/awesome-frontend-interview/issues/26)

</details>

<details>
<summary>CSS 常见布局实现</summary>

#### 参考

- [干货!各种常见布局实现](https://juejin.im/post/5aa252ac518825558001d5de)

</details>

<details>
<summary>CSS 双飞翼布局 & 圣杯布局</summary>

- 双飞翼布局

```html
<body>
<div id="hd">header</div> 
  <div id="middle">
    <div id="inside">middle</div>
  </div>
  <div id="left">left</div>
  <div id="right">right</div>
  <div id="footer">footer</div>
</body>
```
- 圣杯布局

```html
<body>
<div id="hd">header</div>
<div id="bd">
  <div id="middle">middle</div>
  <div id="left">left</div>
  <div id="right">right</div>
</div>
<div id="footer">footer</div>
</body>
```

> 双飞翼布局是在middle的div里又插入一个div，通过调整内部div的margin值，实现中间栏自适应，内容写到内部div中

> 思路：通过 `float` 浮动，脱离文档流，中间 `100%`, 左边 `-100%`, 右边 `-width` 实现在同一行，然后通过 `position` 左右修正距离，并清除浮动

#### 参考

- [CSS 圣杯布局](https://www.jianshu.com/p/ffc6cbfa759b)
- [圣杯布局和双飞翼布局的理解和区别](https://www.cnblogs.com/lovemomo/p/4885866.html)

</details>

<details>
<summary>CSS Grid布局</summary>

#### 参考

- [CSS 网格布局学习指南](https://blog.jirengu.com/?p=990)

</details>

<details>
<summary>CSS Table布局</summary>

#### 参考

- [html5 div布局与table布局](https://blog.csdn.net/csdn9_14/article/details/53177614)

</details>

<details>
<summary>css3新特性</summary>

#### 参考

- [css3新特性](https://juejin.im/post/5a0c184c51882531926e4294)

</details>

<details>
<summary>box-sizing</summary>

> 设置CSS盒模型为标准模型或IE模型。标准模型的宽度只包括content，二IE模型包括border和padding
box-sizing属性可以为三个值之一：

- `content-box`，默认值，只计算内容的宽度，border和padding不计算入width之内
- `padding-box`，padding计算入宽度内
- `border-box`，border和padding计算入宽度之内

> ie盒模型算上border、padding及自身（不算margin），标准的只算上自身窗体的大小 css设置方法如下

```css
/* 标准模型 */
box-sizing:content-box;
 /*IE模型*/
box-sizing:border-box;
```

#### 参考

- [box-sizing是什么](https://juejin.im/post/5a954add6fb9a06348538c0d)

</details>

<details>
<summary>CSS隐藏元素的几种方式</summary>

- display:none
- visibility:hidden
- opacity:0
- height=width=0
- position
- z-index

#### 参考

- [CSS知识总结](https://juejin.im/post/5a954add6fb9a06348538c0d)

</details>

<details>
<summary>Dom元素获得宽高的方式</summary>

- dom.style.width/height

> 这种方式只能取到dom元素内联样式所设置的宽高，也就是说如果该节点的样式是在style标签中或外联的CSS文件中设置的话，通过这种方法是获取不到dom的宽高的。

- dom.currentStyle.width/height

>这种方式获取的是在页面渲染完成后的结果，就是说不管是哪种方式设置的样式，都能获取到。但这种方式只有IE浏览器支持。

- window.getComputedStyle(dom).width/height 　
　
> 这种方式的原理和2是一样的，这个可以兼容更多的浏览器，通用性好一些。

- dom.getBoundingClientRect().width/height 　　

> 这种方式是根据元素在视窗中的绝对位置来获取宽高的

- dom.offsetWidth/offsetHeight 　　

> 最常用的，也是兼容最好的。

#### 参考

- [前端常见面试题汇总](https://www.geekjc.com/ebook/detail/5ba5bcae7143880b09cb4d54)

</details>

<details>
<summary>各种获得宽高的方式</summary>

- 获取屏幕的高度和宽度（屏幕分辨率）： window.screen.height/width
- 获取屏幕工作区域的高度和宽度（去掉状态栏）： window.screen.availHeight/availWidth
- 网页全文的高度和宽度： document.body.scrollHeight/Width
- 滚动条卷上去的高度和向右卷的宽度： document.body.scrollTop/scrollLeft
- 网页可见区域的高度和宽度（不加边线）： document.body.clientHeight/clientWidth
- 网页可见区域的高度和宽度（加边线）： document.body.offsetHeight/offsetWidth

#### 参考

- [前端常见面试题汇总](https://www.geekjc.com/ebook/detail/5ba5bcae7143880b09cb4d54)

</details>

<details>
<summary>CSS 属性是否区分大小写</summary>

> 不区分。 HTML，CSS都对大小写不敏感，但为了更好的可读性和团队协作一般都小写，而在XHTML 中元素名称和属性是必须小写的

#### 参考

- [Front-end-Web-Development-Interview-Question](https://github.com/paddingme/Front-end-Web-Development-Interview-Question)

</details>

<details>
<summary>rgba和opacity的透明效果有什么不同</summary>

> rgba和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，

> 而rgba只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）

#### 参考

- [rgba()和opacity的透明效果有什么不同](http://www.cnblogs.com/coco1s/p/4034937.html)

</details>

<details>
<summary>描述一个"reset"的CSS文件并如何使用它。知道normalize.css吗？你了解他们的不同之处</summary>

> 重置样式非常多，凡是一个前端开发人员肯定有一个常用的重置CSS文件并知道如何使用它们。他们是盲目的在做还是知道为什么这么做呢？原因是不同的浏览器对一些元素有不同的默认样式，如果你不处理，在不同的浏览器下会存在必要的风险，或者更有戏剧性的性发生。

> 你可能会用 [Normalize](http://necolas.github.io/normalize.css/) 来代替你的重置样式文件。它没有重置所有的样式风格，但仅提供了一套合理的默认样式值。既能让众多浏览器达到一致和合理，但又不扰乱其他的东西（如粗体的标题）

#### 参考

- [描述一个"reset"的CSS文件并如何使用它。知道normalize.css吗？你了解他们的不同之处](http://www.cnblogs.com/coco1s/p/4034937.html)

</details>

<details>
<summary>什么是CSS 预处理器 / 后处理器？</summary>

- 预处理器例如：LESS、Sass、Stylus，用来预编译Sass或less，增强了css代码的复用性，
    还有层级、mixin、变量、循环、函数等，具有很方便的UI组件模块化开发能力，极大的提高工作效率。

- 后处理器例如：PostCSS，通常被视为在完成的样式表中根据CSS规范处理CSS，让其更有效；目前最常做的
  是给CSS属性添加浏览器私有前缀，实现跨浏览器兼容性的问题。

#### 参考

- [Questions-and-Answers](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers)

</details>

<details>
<summary>移动设备rem适配？</summary>

#### 参考

- [一步步教你使用rem适配不同屏幕的移动设备](https://www.cnblogs.com/dannyxie/p/6640903.html)
- [（淘宝无限适配）手机端rem布局详解](https://www.cnblogs.com/well-nice/p/5509589.html)

</details>

</details>

<details>
<summary>CSS中position属性介绍(新增sticky)？</summary>

#### 参考

- [CSS中position属性介绍(新增sticky)](https://www.cnblogs.com/s1nker/p/4835079.html)
- [HTML DOM position 属性](http://www.w3school.com.cn/jsref/prop_style_position.asp)

</details>

