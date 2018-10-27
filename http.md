# Http

<details>
<summary>XMLHttpRequest</summary>

#### 参考

- [你真的会使用XMLHttpRequest吗](https://segmentfault.com/a/1190000004322487)

</details>

<details>
<summary>浏览器输入URL到展示页面过程</summary>

- 一. 网络通信
  1. 在浏览器中输入URL

    ```
    用户输入URL,例如https://www.baidu.com. 其中https为协议, www.baidu.com为网络地址
    ```

  2. 应用层DNS解析域名

  ```
  客户端先检查本地是否有对应的IP地址,若找到侧返回对应的IP地址.若没有找到则请求上级DNS服务器,直到找到根节点
  ```

  3. 客户端发送https请求

  ```
  HTTP请求包括请求报头和请求主体两个部分,其中请求报头包含了重要的信息,包括请求的方法(GET/ POST), 目标URL, 遵循的协议(http/https/ftp...),返回的信息是否需要缓存,以及客户端是否发送cookie等
  ```

  4. 打开一个socket与目标IP地址, 端口建立TCP连接

  ```
  位于传输层的TCP协议为传输报文提供可靠的字节流服务.它为了方便传输,将大块的数据分割成以报文段为单位的数据进行管理,并为他们编号,方便服务器接受的时候能准确的还原报文信息. TCP协议通过"三次握手"等方法保证传输的可靠性.  

   "三次握手"的过程,发送端先发送一个带SYN标志的数据包给接收端,在一定的延迟时间内等待接受的回复. 接收端收到数据包后,传回一个带SYN/ACK标志的数据包以示传达确认信息. 接收方收到后再发送一个带ACK标志的数据包给接收方以示握手成功. 在这个过程中,如果发送端在规定延迟时间内没有收到回复则默认接受方没有收到请求,而再次发送,知道收到回复为止.
  ```

  5. 网络层IP协议查询MAC地址

  ```
  IP协议的作用就是把TCP分割好的各种数据包传送给接收方.而要保证确实能传到接收方还需要接收方的MAC地址,也就是物理地址. IP地址和MAC地址是一一对应的关系,一个网络设备的IP地址可以更换,但是MAC地址一般是固定不变的. ARP协议可以将IP地址解析成对应的MAC地址. 当通讯的双方不在同一个局域网时,需要多次中转才能到达最终的目标,在中转的过程中需要通过下一个中转站的MAC地址来搜索下一个中转目标.
  ```

  6. 数据到达数据链层(tcp建立连接后,发送http请求)

  ```
  再找到对方的MAC地址后,就将数据发送到数据链层传输. 这时,客户端发送请求的阶段结束.
  ```

  7. 服务器接受数据

  ```
  接收端的服务器在链路层接收到数据包,在层层向上直到应用层. 这过程中包括在运输层通过TCP协议将分段的数据包重新组成原来的HTTP请求报文.
  ```

  8. 服务器响应请求

  ```
  服务器接收到客户端发送的请求,查找客户端请求的资源,并返回响应报文,响应报文中包括一个重要的信息----状态码.状态码有三位数字组成,其中比较常见的200 ok表示请求成功. 301表示永久重定向,即请求的资源已经永久的转移到新的位置. 再返回301状态码的同时,响应报文也会附带重定向的URL,客户端接收到后将http请求的URL做相应的改变再重新发送. 404 not found 表示客户端请求的资源找不到.
  ```

  9. 服务器返回相应的文件

  ```
  对服务器响应进行解码,根据资源类型决定如何处理, 页面渲染.
  ```

- 二. 页面渲染

  ```
  现在浏览器渲染页面的过程是这样的:解析HTML以构建DOM树->构建渲染树->布局渲染树->绘制渲染树

    DOM树是由HTML文件中的标签排列组成,渲染树是在DOM树中加入CSS或HTML中的style样式而形成的.渲染树只包含需要显示在页面中的DOM元素,像<head>元素或display属性值为none的元素都不在渲染树中.

    在浏览器还没有接收到完整的HTML文件时,它就开始渲染页面了,在遇到外部链入的脚本标签或样式标签或图片,会再次发送http请求重复上述的步骤. 在收到CSS文件后会对已经渲染的页面重新渲染,加入他们应有的样式,图片文件加载完成立刻显示在相应的位置. 在这一过程可能会触发页面的重绘或者重排.
  ```

#### 参考

- [浏览器输入URL到展示页面过程](https://www.jianshu.com/p/0a2c35e8e2b7)
- [页面从输入URL到展现发生了什么](https://huruji.github.io/FE-Interview/#/docs/NetWork?id=_1%E9%A1%B5%E9%9D%A2%E4%BB%8E%E8%BE%93%E5%85%A5url%E5%88%B0%E5%B1%95%E7%8E%B0%E5%8F%91%E7%94%9F%E4%BA%86%E4%BB%80%E4%B9%88)
- [从浏览器地址栏输入url到显示页面的步骤(以HTTP为例)](https://github.com/HerbertKarajan/Fe-Interview-questions/tree/master/23-FE-interview-master#%E4%BB%8E%E6%B5%8F%E8%A7%88%E5%99%A8%E5%9C%B0%E5%9D%80%E6%A0%8F%E8%BE%93%E5%85%A5url%E5%88%B0%E6%98%BE%E7%A4%BA%E9%A1%B5%E9%9D%A2%E7%9A%84%E6%AD%A5%E9%AA%A4%E4%BB%A5http%E4%B8%BA%E4%BE%8B)

</details>

<details>
<summary>JS跨域问题</summary>

> 跨域问题是由于javascript语言安全限制中的同源策略造成的.同源策略是指一段脚本只能读取来自同一来源的窗口和文档的属性,这里的同一来源指的是主机名、协议和端口号的组合.

#### 解决方式

- 跨域资源共享（CORS）
- jsonp
- iframe
- postMessage

#### 参考

- [JS 跨域问题常见的五种解决方式](https://www.cnblogs.com/imwtr/p/4764123.html)

</details>

<details>
<summary>将静态资源放在其他域名的目的是什么</summary>

> 在请求这些静态资源的时候不会发送cookie，节省了流量，需要注意的是cookie是会发送给子域名的（二级域名），所以这些静态资源是不会放在子域名下的， 而是单独放在一个单独的主域名下。同时还有一个原因就是浏览器对于一个域名会有请求数的限制，这种方法可以方便做CDN

- CDN缓存更方便 
- 突破浏览器并发限制 
- 节约cookie带宽 
- 节约主域名的连接数，优化页面响应速度 
- 防止不必要的安全问题

#### 参考

- [将静态资源放在其他域名的目的是什么](https://huruji.github.io/FE-Interview/#/docs/JavaScript?id=_41%E5%B0%86%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E6%94%BE%E5%9C%A8%E5%85%B6%E4%BB%96%E5%9F%9F%E5%90%8D%E7%9A%84%E7%9B%AE%E7%9A%84%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)

</details>

<details>
<summary>cookie和session的异同</summary>

> cookie和session都可以用来存储用户信息，cookie存放于客户端，session存放于服务端，因为cookie存放于客户端 有可能被窃取，因此cookie一般用来存放不敏感的信息，如用户设置的网站主题等，敏感的信息采用session存储，如用户 的登陆信息，session可以存放于文件、数据库、内存中都可以，cookie可以服务端响应的时候设置，也可以客户端通过js设置, cookie会在请求时在http首部发送给客户端，cookie一般在客户端有大小限制，一般为4k。

#### 参考

- [cookie和session的异同](https://huruji.github.io/FE-Interview/#/docs/NetWork?id=_2cookie%E5%92%8Csession%E7%9A%84%E5%BC%82%E5%90%8C)

</details>

<details>
<summary>常见的网页性能优化方法</summary>

- 减少HTTP请求
> 使用雪碧图、内联图片，合并脚本和样式表。
- 使用内容分发网络（CDN）
- 添加Expires头
- 压缩组件
> 压缩样式表和脚本，开启gzip压缩大概减少70%的大小
- 样式表放在顶部
- 将脚本放在底部
- 避免CSS表达式
- 使用外部JavaScript和CSS
- 减少DNS查找
- 精简JavaScript
- 避免重定向
- 网站中除了域名首页外缺少斜杠将引起301重定向，个人测试工作室网站这个重定向消耗的时间在30ms左右
- 删除重复脚本
- 配置ETag
- 使Ajax可缓存

#### 参考

- [常见的网页性能优化方法](https://huruji.github.io/FE-Interview/#/docs/Performance)

</details>

<details>
<summary>XML和JSON的区别？</summary>

- 数据体积方面。

JSON相对于XML来讲，数据的体积小，传递的速度更快些。

- 数据交互方面。

JSON与JavaScript的交互更加方便，更容易解析处理，更好的数据交互。

- 数据描述方面。

JSON对数据的描述性比XML较差。

- 传输速度方面。

JSON的速度要远远快于XML。

#### 参考

- [Fe-Interview-questions](https://github.com/HerbertKarajan/Fe-Interview-questions/tree/master/21-Front-end-Interview-questions)

</details>

<details>
<summary>IndexDB 、WebSql、LocalStorage、SessionStorage、Cookie</summary>

> 虽然在HTML5 WebStorage介绍了html5本地存储的 `Local Storage` 和 `Session Storage` (其API是同步)，这两个是以 `键值对存储` 的解决方案，存储少量数据结构很有用，**但是对于大量结构化数据就无能为力了，灵活大不够强大**。我们经常在数据库中处理大量结构化数据，html5引入 `Web SQL Database` 概念，它使用 SQL 来操纵客户端数据库的 API，规范中使用的方言是SQLlite; `IndexedDB` 是HTML5规范里新出现的浏览器里内置的数据库。对于在浏览器里存储数据，你可以使用cookies或local storage，但它们都是比较简单的技术，而IndexedDB提供了类似 *数据库风格的数据存储和使用方式* 。存储在IndexedDB里的数据是  **永久保存**

> `Local Storage` 和 `Session Storage` API是同步的。  
> `Web SQL` 和 `IndexedDB` API是异步的

#### 参考

- [HTML5本地存储——Web SQL Database与indexedDB](https://www.cnblogs.com/hoboStage/p/5099637.html)

</details>

<details>
<summary>http状态码有那些？分别代表是什么意思？</summary>

- 简单版

```
100  Continue	继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
200  OK 		正常返回信息
201  Created  	请求成功并且服务器创建了新的资源
202  Accepted 	服务器已接受请求，但尚未处理
301  Moved Permanently  请求的网页已永久移动到新位置。
302 Found  		临时性重定向。
303 See Other  	临时性重定向，且总是使用 GET 请求新的 URI。
304  Not Modified 自从上次请求后，请求的网页未修改过。

400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
401 Unauthorized 请求未授权。
403 Forbidden  	禁止访问。
404 Not Found  	找不到如何与 URI 相匹配的资源。

500 Internal Server Error  最常见的服务器端错误。
503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。
```

- 完整版

```
1**(信息类)：表示接收到请求并且继续处理
100——客户必须继续发出请求
101——客户要求服务器根据请求转换HTTP协议版本

2**(响应成功)：表示动作被成功接收、理解和接受
200——表明该请求被成功地完成，所请求的资源发送回客户端
201——提示知道新文件的URL
202——接受和处理、但处理未完成
203——返回信息不确定或不完整
204——请求收到，但返回信息为空
205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
206——服务器已经完成了部分用户的GET请求

3**(重定向类)：为了完成指定的动作，必须接受进一步处理
300——请求的资源可在多处得到
301——本网页被永久性转移到另一个URL
302——请求的网页被转移到一个新的地址，但客户访问仍继续通过原始URL地址，重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。
303——建议客户访问其他URL或访问方式
304——自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容，代表上次的文档已经被缓存了，还可以继续使用
305——请求的资源必须从服务器指定的地址得到
306——前一版本HTTP中使用的代码，现行版本中不再使用
307——申明请求的资源临时性删除

4**(客户端错误类)：请求包含错误语法或不能正确执行
400——客户端请求有语法错误，不能被服务器所理解
401——请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
HTTP 401.1 - 未授权：登录失败
　　HTTP 401.2 - 未授权：服务器配置问题导致登录失败
　　HTTP 401.3 - ACL 禁止访问资源
　　HTTP 401.4 - 未授权：授权被筛选器拒绝
HTTP 401.5 - 未授权：ISAPI 或 CGI 授权失败
402——保留有效ChargeTo头响应
403——禁止访问，服务器收到请求，但是拒绝提供服务
HTTP 403.1 禁止访问：禁止可执行访问
　　HTTP 403.2 - 禁止访问：禁止读访问
　　HTTP 403.3 - 禁止访问：禁止写访问
　　HTTP 403.4 - 禁止访问：要求 SSL
　　HTTP 403.5 - 禁止访问：要求 SSL 128
　　HTTP 403.6 - 禁止访问：IP 地址被拒绝
　　HTTP 403.7 - 禁止访问：要求客户证书
　　HTTP 403.8 - 禁止访问：禁止站点访问
　　HTTP 403.9 - 禁止访问：连接的用户过多
　　HTTP 403.10 - 禁止访问：配置无效
　　HTTP 403.11 - 禁止访问：密码更改
　　HTTP 403.12 - 禁止访问：映射器拒绝访问
　　HTTP 403.13 - 禁止访问：客户证书已被吊销
　　HTTP 403.15 - 禁止访问：客户访问许可过多
　　HTTP 403.16 - 禁止访问：客户证书不可信或者无效
HTTP 403.17 - 禁止访问：客户证书已经到期或者尚未生效
404——一个404错误表明可连接服务器，但服务器无法取得所请求的网页，请求资源不存在。eg：输入了错误的URL
405——用户在Request-Line字段定义的方法不允许
406——根据用户发送的Accept拖，请求资源不可访问
407——类似401，用户必须首先在代理服务器上得到授权
408——客户端没有在用户指定的饿时间内完成请求
409——对当前资源状态，请求不能完成
410——服务器上不再有此资源且无进一步的参考地址
411——服务器拒绝用户定义的Content-Length属性请求
412——一个或多个请求头字段在当前请求中错误
413——请求的资源大于服务器允许的大小
414——请求的资源URL长于服务器允许的长度
415——请求资源不支持请求项目格式
416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求长。

5**(服务端错误类)：服务器不能正确执行一个正确的请求
HTTP 500 - 服务器遇到错误，无法完成请求
　　HTTP 500.100 - 内部服务器错误 - ASP 错误
　　HTTP 500-11 服务器关闭
　　HTTP 500-12 应用程序重新启动
　　HTTP 500-13 - 服务器太忙
　　HTTP 500-14 - 应用程序无效
　　HTTP 500-15 - 不允许请求 global.asa
　　Error 501 - 未实现
HTTP 502 - 网关错误
HTTP 503：由于超载或停机维护，服务器目前无法使用，一段时间后可能恢复正常
```

#### 参考

- [Questions-and-Answers](https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers)

</details>