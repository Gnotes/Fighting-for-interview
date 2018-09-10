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