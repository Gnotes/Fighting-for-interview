# NodeJs


<details>
<summary>Node.js 中的异常捕获</summary>

捕获异常一般三种：

- uncaughtException
- `回调函数前` try-catch
- domian捕获(domain模块,这是node自带的一个模块)

```js
process.on('uncaughtException', (err)=>{

})
```

```js
router.all("/api/xxx", async(req, res) => {
  try{
    res.end(); // 成功返回内容
  } catch(err){
    // 此处希望throw err 让server接收并处理，但是会报错
    res.end(); // 失败返回内容
  }
});
```

```js
var domain = require('domain');

uncaughtExceptionMiddleWare = (req, res, next)=>{
  var _domain = domain.create();
  _domain.on('error', function (err) {  // 下面抛出的异常在这里被捕获,触发此事件
      console.log('捕获到错误');
      res.send(500, err.stack);         // 成功给用户返回了 500
  });
  _domain.run(next);
}

app.use(uncaughtExceptionMiddleWare);
```

#### 参考

- [Express中全局异常处理](https://segmentfault.com/q/1010000009295251?_ea=1884908)
- [node.js 使用domain模块捕获异步回调中的异常](http://www.cnblogs.com/ysk123/p/9848612.html)

</details>