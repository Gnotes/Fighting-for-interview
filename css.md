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