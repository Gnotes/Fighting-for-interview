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
通过生成一个同等宽度 并隐藏的盒模型（div 或者 p） ，然后将文字放入，通过计算行高的方式，递归截取相应长度的文字
```

#### 参考

- [awesome-frontend-interview](https://github.com/zanjs/awesome-frontend-interview/issues/77)

</details>

<details>
<summary>link与@import 的区别</summary>

- 属性功能差别

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

