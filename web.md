# Web

<details>
<summary>浏览器多个标签之间共享数据和通信</summary>

- localstorge

```
localstorage是浏览器多个标签共用的存储空间，所以可以用来实现多标签之间的通信
```

类比： *`sessionStorage` 是会话级的存储空间，每个标签页都是单独的*，**不能用于共享数据**

- cookie

```
由于 Cookies 是在同域可读的，所以在页面 B 改变 Cookies 的值，页面 A 自然是可以拿到的
```

- SharedWorker

```
普通的 webworker 直接使用new Worker()即可创建，这种webworker是当前页面专有的

然后还有种共享worker(SharedWorker)，这种是可以多个标签页、iframe共同使用的，SharedWorker可以被多个window共同使用，但必须保证这些标签页都是同源的(相同的协议，主机和端口号)
```

#### 参考

- [简书](https://www.jianshu.com/p/5f0e6d8fd36a)

</details>

<details>
<summary>WebSocket通讯</summary>

```
WebSocket是HTML5新增的协议，它的目的是在服务器和浏览器之间建立一个不受限的双向通信的通道，如: 服务器可以在任意时刻发送消息给浏览器  
```

#### 特点

- 建立在 TCP 协议之上，服务器端的实现比较容易。
- 与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。
- 数据格式比较轻量，性能开销小，通信高效。
- 可以发送文本，也可以发送二进制数据。
- 没有同源限制，客户端可以与任意服务器通信。
- 协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。

其他实现方式：  

1. 轮询  
2. [Comet](https://www.cnblogs.com/imstudy/p/5696033.html)
    > 基于HTTP长连接的Web端实时通信技术，Comet本质上也是轮询，但是在没有消息的情况下，服务器先拖一段时间，等到有消息了再回复,这个机制暂时地解决了实时性问题，但是它带来了新的问题：以多线程模式运行的服务器会让大部分线程大部分时间都处于挂起状态，极大地浪费服务器资源。另外，一个HTTP连接在长时间没有数据传输的情况下，链路上的任何一个网关都可能关闭这个连接，而网关是我们不可控的，这就要求Comet连接必须定期发一些ping数据表示连接“正常工作”


#### 参考

- [简书](https://www.jianshu.com/p/5f0e6d8fd36a)
- [Comet技术详解：基于HTTP长连接的Web端实时通信技术](https://www.cnblogs.com/imstudy/p/5696033.html)

</details>