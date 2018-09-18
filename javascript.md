# JavaScript

<details>
<summary>typeof 与 instanceof</summary>

> `typeof` 和 `instanceof` 常用来判断一个变量类型

typeof 一般只能返回如下几个结果: 

- number
- string
- boolean
- object
- function
- undefined

> `instanceof` 运算符判断是否属于某个构造的实例

#### 参考

- [JS中typeof与instanceof的区别](https://www.cnblogs.com/Trr-984688199/p/6180040.html)
- [JavaScript专题之类型判断(上)](https://github.com/mqyqingfeng/Blog/issues/28)
- [JavaScript专题之类型判断(下)](https://github.com/mqyqingfeng/Blog/issues/30)

</details>

<details>
<summary>如何判断一个变量是对象还是数组</summary>

- typeof + length

由于 `typeof` 都返回 `object`，因此需要加上 `length` 属性判断

```js
(o)=>{
  if(typeof o === 'object'){
    if( typeof o.length === 'number' ){
      return 'Array';
    } else {
      return 'Object';
    }
  }
}
```

- instanceof

```js
var obj = {};
var arr = [];

obj instanceof Object
arr instanceof Array
```

由于数组也是 `Object`，因此在判断的时候，需要先判断是否为 Array，然后才是 Object

```js
(o)=>{
  if(o instanceof Array){
    return 'Array';
  } else if(o instanceof Object){
    return 'Object';
  }
}
```

- constructor

```js
(o)=>{
  if(o.constructor === Array){
    return 'Array';
  } else if(o.constructor === Object){
    return 'Object';
  }
}
```

- toString()

数组原型和对象原型定义的toString()方法不同

```js
(o)=>{
  if(Object.prototype.toString.call(o) === '[object Array]'){
    return 'Array';
  } else if(Object.prototype.toString.call(o) === '[object Object]'){
    return 'Object';
  }
}
```

- Array.isArray()

```js
(o)=>{
  if(Array.isArray(o)){
    return 'Array';
  }
  return 'Object';
}
```

#### 参考

- [JS中typeof与instanceof的区别](https://www.cnblogs.com/Trr-984688199/p/6180040.html)
- [判断一个变量类型是数组还是对象](https://www.cnblogs.com/Walker-lyl/p/5597547.html)

</details>

<details>
<summary>常用的 Javascript 设计模式</summary>

> 设计模式：一套被反复使用、经过分类编目的、代码设计经验的总结

- 单体模式
- 工厂模式
- 单例模式
- 观察者模式（发布订阅模式）
- 策略模式
- 模板模式
- 代理模式
- 外观模式

#### 单体模式（不是单例）

> 只能被实例化一次，将一批相关的属性和方法组织在一起的对象

```js
const Singleton = {
  attribute: true,
  method1: ()=>{},
  method2: ()=>{}
};
```

#### 工厂模式

> 提供创建对象的接口，意思就是根据领导（调用者）的指示（参数），生产相应的产品（对象）

- `简单工厂模式`：使用一个类，通常为单体，来生成实例。
- `复杂工厂模式`：将其成员对象的实列化推到子类中，子类可以重写父类接口方法以便创建的时候指定自己的对象类型

```js
 // 简单工厂模式
const XMLHttpFactory = function(){};

XMLHttpFactory.createXMLHttp = function(){
  let XMLHttp = null;
  if (window.XMLHttpRequest){
    XMLHttp = new XMLHttpRequest()
  } else if (window.ActiveXObject){
    XMLHttp = new ActiveXObject("Microsoft.XMLHTTP")
  }
  return XMLHttp;
}

// XMLHttpFactory.createXMLHttp()这个方法根据当前环境的具体情况返回一个XHR对象。
const AjaxHander = function(){
  const XMLHttp = XMLHttpFactory.createXMLHttp();
}
```

```js
// 复杂工厂模式
const XMLHttpFactory =function(){};

XMLHttpFactory.prototype = {
　　// 如果真的要调用这个方法会抛出一个错误，它不能被实例化，只能用来派生子类
　　createFactory:function(){
  　　throw new Error('我是一个抽象方法，不能直接调用');
　　}
}

const XHRHandler = function(){}; // 定义一个子类

// 子类继承父类原型方法
extend( XHRHandler , XMLHttpFactory );

XHRHandler.prototype = new XMLHttpFactory(); // 把超类原型引用传递给子类,实现继承
XHRHandler.prototype.constructor = XHRHandler; // 重置子类原型的构造器为子类自身

//重新定义createFactory 方法
XHRHandler.prototype.createFactory = function(){
  var XMLHttp = null;
  if (window.XMLHttpRequest){
    XMLHttp = new XMLHttpRequest();
  } else if (window.ActiveXObject){
    XMLHttp = new ActiveXObject("Microsoft.XMLHTTP")
  }
  return XMLHttp;
}
```

#### 单例模式

> 单例模式定义了一个对象的创建过程，此对象只有一个单独的实例

```js
var single = (function(){
  var instance;

  function getInstance(){
　　// 如果该实例存在，则直接返回，否则就对其实例化
    if( instance === undefined ){
        instance = new Constructor();
    }
    return instance;
  }

  function Constructor(){
    // ... 生成单例的构造函数的代码
  }

  return {
      getInstance : getInstance
  }
})();
```

#### 观察者模式

> 定义对象间的一种一对多的依赖关系，以便当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并自动刷新，也被称为是发布订阅模式。  
它需要一种高级的抽象策略，以便订阅者能够彼此独立地发生改变，而发行方能够接受任何有消费意向的订阅者  

```js
var pubsub = {};   // 定义发布者

(function (q) {

  var list = [],  //回调函数存放的数组，也就是记录有多少人订阅了我们东西
    subUid = -1;

  // 发布消息,遍历订阅者
  q.publish = function (type, content) {
    // type 为文章类型，content为文章内容

    // 如果没有人订阅，直接返回
    if (!list[type]) {

      return false;
    }

    setTimeout(function () {
      var subscribers = list[type],
        len = subscribers ? subscribers.length : 0;

      while (len--) {
        // 将内容注入到订阅者那里
        subscribers[len].func(type, content);
      }
    }, 0);

    return true;

  };
  //订阅方法，由订阅者来执行
  q.subscribe = function (type, func) {
    // 如果之前没有订阅过
    if (!list[type]) {
      list[type] = [];
    }

    // token相当于订阅者的id，这样的话如果退订，我们就可以针对它来知道是谁退订了。
    var token = (++subUid).toString();
    // 每订阅一个，就把它存入到我们的数组中去
    list[type].push({
      token: token,
      func: func
    });
    return token;
  };
  //退订方法
  q.unsubscribe = function (token) {
    for (var m in list) {
      if (list[m]) {
        for (var i = 0, j = list[m].length; i < j; i++) {
          if (list[m][i].token === token) {
            list[m].splice(i, 1);
            return token;
          }
        }
      }
    }
    return false;
  };

}(pubsub));

//将订阅赋值给一个变量，以便退订
var xing = pubsub.subscribe('JavaScript', function (type, content) {
  console.log('xing订阅的' + type + ": 内容内容为：" + content);
});

// 发布通知
pubsub.publish('JavaScript', '关于js的内容');
// 退订
pubsub.unsubscribe(girlA);
```

#### 策略模式

> 策略模式指的是定义一些列的算法，把他们一个个封装起来，目的就是将算法的使用与算法的实现分离开来。说白了就是以前要很多判断的写法，现在把判断里面的内容抽离开来，变成一个个小的个体

- Before

```js
function Price(personType, price) {
  //vip 5 折
  if (personType == 'vip') {
    return price * 0.5;
  } 
  else if (personType == 'old'){ //老客户 3 折
    return price * 0.3;
  } else {
    return price; //其他都全价
  }
}
```

- After

```js
// 对于vip客户
function vipPrice() {
  this.discount = 0.5;
}

vipPrice.prototype.getPrice = function (price) {
  　　return price * this.discount;
}
// 对于老客户
function oldPrice() {
  this.discount = 0.3;
}

oldPrice.prototype.getPrice = function (price) {
  return price * this.discount;
}
// 对于普通客户
function Price() {
  this.discount = 1;
}

Price.prototype.getPrice = function (price) {
  return price;
}

// 上下文，对于客户端的使用
function Context() {
  this.name = '';
  this.strategy = null;
  this.price = 0;
}

// strategy 不同客户对应的策略
Context.prototype.set = function (name, strategy, price) {
  this.name = name;
  this.strategy = strategy;
  this.price = price;
}
Context.prototype.getResult = function () {
  console.log(this.name + ' 的结账价为: ' + this.strategy.getPrice(this.price));
}

var context = new Context();
var vip = new vipPrice();
context.set('Vip', vip, 200);
context.getResult(); // Vip 的结账价为: 100
```

#### 模板模式

> 将一些公共方法封装到父类，子类可以继承这个父类，并且可以在子类中重写父类的方法，从而实现自己的业务逻辑

```js
var Interview = function () { };
// 笔试
Interview.prototype.writtenTest = function () {
  console.log("父类前端笔试");
};
// 技术面试
Interview.prototype.technicalInterview = function () {
  console.log("父类技术面试");
};

// 代码初始化
Interview.prototype.init = function () {
  this.writtenTest();
  this.technicalInterview();
};

// 重写父类方法，继承父类其他方法。
var AliInterview = function () { };
// 重置原型，即继承
AliInterview.prototype = new Interview();

// 子类重写方法 实现自己的业务逻辑
AliInterview.prototype.writtenTest = function () {
  console.log("子类前端面试");
}
var AliInterview = new AliInterview();
AliInterview.init();

// 子类前端笔试
// 父类技术面试
```

#### 代理模式

> 代理模式的中文含义就是帮别人做事，javascript的解释为：把对一个对象的访问, 交给另一个代理对象来操作.

```js
// 补打卡事件
var fillOut = function (lateDate) {
  this.lateDate = lateDate;
};

// Boss
var Boss = function (fillOut) {
  this.state = function (isSuccess) {
    console.log("忘记打卡的日期为：" + fillOut.lateDate + ", 补打卡状态：" + isSuccess);
  }
};
// 秘书代理boss 完成补打卡审批
var Secretary = function (fillOut) {
  this.state = function (isSuccess) {
    (new Boss(fillOut)).state(isSuccess); // 替Boss审批
  }
};

// 调用方法：
var secretary = new Secretary(new fillOut("2016-9-11"));
secretary.state("补打卡成功");
```

#### 外观模式

> 通过编写一个单独的函数，来简化对一个或多个更大型的，可能更为复杂的函数的访问。也就是说可以视外观模式为一种简化某些内容的手段，说白了，外观模式就是一个函数，封装了复杂的操作

比如一个跨浏览器的ajax调用

```js
function ajax(type, url, callback, data) {
  // 根据当前浏览器获取对ajax连接对象的引用
  var xhr = (function () {
    if (window.XMLHttpRequest) {
      return new XMLHttpRequest(); // 所有现代浏览器所使用的标准方法
    } else if (window.ActiveXObject) {
      return new ActiveXObject(); // 较老版本的internet Explorer兼容
    }
    // 如果没能找到相关的ajax连接对象，则跑出一个错误。
    throw new Error("Ajax not support in this browser.")
  }()),
    STATE_LOADED = 4,
    STATUS_OK = 200;
  // 一但从服务器收到表示成功的相应消息，则执行所给定的回调方法
  xhr.onreadystatechange = function{
    if (xhr.readyState !== STATE_LOADED) {
      return;
    }
    if (xhr.state == STATUS_OK) {
      callback(xhr.responseText);
    }
  }

  // 使用浏览器的ajax连接对象来向所给定的URL发出相关的调用
  xhr.open(type.toUpperCase(), url);
  xhr.send(data);
}

// 使用方法
ajax("get", "/api/fetch", function (result) {
  alert('收到的数据为：' + result);
})
```

#### 参考

- [常用的javascript设计模式](https://www.cnblogs.com/xianyulaodi/p/5827821.html)

</details>

<details>
<summary> JavaScript等号判断相等流程 </summary>

#### ===运算符判断相等的流程是怎样的

- 类型不同，不等
- null，undefined，boolean，number这四个类型的只要值(数值)相等，就相等，0 === 0 //true
- 只要其中有一个为NAN，则不等
- string类型，长度/内容/编码不同，都是不等，相同位置包含相同的16位，相等
- 指向相同的对象，数组，函数，则相等，若指向不同对象，不等

#### ==运算符判断相等的流程是怎样的

- 若类型不同，则按===规则判断
- 类型不同，则启用隐式类型转换
- 有NAN，一律返回false
- 有布尔类型，布尔类型转换成数字比较
- 有string类型，两种情况： 1. 对象，对象用toString方法转换成string相比。2.数字，string类型转换成数字进行比较
- null和undefined不会相互转换，相等
- 有数字类型，和对象相比，对象用valueof转换成原始值进行比较
- 其他情况，一律返回false

#### 参考

- [javascript等号判断相等流程](https://segmentfault.com/a/1190000006813184)

</details>

<details>
<summary> undefined,null,NaN的区别 </summary>

#### 类型分析

> JavaScript中的数据类型有 undefined,boolean,number,string,object等5种，前4种为原始类型，第5种为引用类型

```js
var a1;
var a2 = true;
var a3 = 1;
var a4 = "Hello";
var a5 = new Object();
var a6 = null;
var a7 = NaN;
var a8 = undefined;

typeof a  // undefined
typeof a1 // undefined
typeof a2 // boolean
typeof a3 // number
typeof a4 // string
typeof a5 // object
typeof a6 // object
typeof a7 // number
```

可以看出 `未定义的值` 和定义未赋值的为 `undefined`，`null` 是一种特殊的 `object` ,`NaN` 是一种特殊的 `number`

#### 比较运算

```js
var a1;  // undefined
var a2 = null;
var a3 = NaN;

a1 == a2 // true
a1 != a2 // false
a1 == a3 // false
a1 != a3 // true
a2 == a3 // false
a2 != a3 // true
a3 == a3 // false
a3 != a3 // true
```

1）`undefined` 与 `null` 是相等  
2）`NaN` 与任何值都不相等，与自己也不相等  

> null 表示无值，而 undefined 表示一个未声明的变量，或已声明但没有赋值的变量，或一个并不存在的对象属性

#### 参考

- [undefined,null,NaN的区别](https://www.jb51.net/article/44472.htm)

</details>

<details>
<summary>立即执行函数表达式（IIFE）</summary>

#### 参考

- [立即执行函数表达式（IIFE）](https://segmentfault.com/a/1190000003985390)

</details>

<details>
<summary>事件委托</summary>

#### 参考

- [事件委托](https://www.cnblogs.com/liugang-vip/p/5616484.html)

</details>

<details>
<summary>script 标签的defer、async的区别</summary>

> 由于解释器在解析执行js代码期间会阻塞页面其余部分的渲染，对于存在大量js代码的页面来说会导致浏览器出现长时间的空白和延迟

- `defer` 和 `async` 在网络加载过程是一致的，都是异步加载并执行的
- 两者的区别在于脚本加载完成之后何时执行，`defer` 执行需要等到文档所有元素解析完成之后，DOMContentLoaded事件触发执行之前，而 `async` 是加载完成后立即执行
- 如果存在多个有 `defer` 属性的脚本，那么它们是按照 `加载顺序` 执行脚本的；而对于 `async`，它的加载和执行是紧紧挨着的，无论声明顺序如何，只要加载完成就立刻执行，它对于应用脚本用处不大，因为它完全不考虑依赖

#### 参考

- [script标签中defer和async属性的区别](https://www.cnblogs.com/neusc/archive/2016/08/12/5764162.html)

</details>

<details>
<summary>js创建对象的多种方式</summary>

- 对象字面量
- 内置构造函数
- 构造函数模式
- 原型
- class

#### 参考

- [js创建对象的多种方式及优缺点](https://www.cnblogs.com/cythia/p/6958021.html)
- [JavaScript深入之创建对象的多种方式以及优缺点](https://github.com/mqyqingfeng/Blog/issues/15)

</details>

<details>
<summary>js常用继承方式</summary>

- 原型

```js
function Parent() { }
function Child() {}

Child.prototype = new Parent();
```

- 构造函数

```js
function Parent(name, age) {
  this.name = name;
  this.age = age;
}

function Child(name, age) {
  Parent.call(this, name, age); // 或者apply
}
```

- extends

```js
class Parent (){ }
class Child extends Parent { }
```

#### 参考

- [js中实现继承的几种方式](https://www.cnblogs.com/diligentYe/p/6413450.html)
- [JavaScript深入之继承的多种方式和优缺点](https://github.com/mqyqingfeng/Blog/issues/16)

</details>

<details>
<summary>不规则图形事件处理</summary>

**热区处理**

</details>

<details>
<summary>闭包</summary>

> 一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分

> 闭包 = 函数 + 函数能够访问的自由变量  
> 自由变量是指在函数中使用的，但既不是函数参数也不是函数的局部变量的变量

**从技术的角度讲，所有的JavaScript函数都是闭包**

```js
var a = 1;

function foo() {
  console.log(a);
}

foo();
```

> foo 函数可以访问变量 a，但是 a 既不是 foo 函数的局部变量，也不是 foo 函数的参数，所以 a 就是自由变量。  
> 那么，函数 foo + foo 函数访问的自由变量 a 不就是构成了一个闭包嘛

因此这也就能解释为什么 `所有的JavaScript函数都是闭包`

#### 参考

- [全面理解Javascript闭包和闭包的几种写法及用途](https://www.cnblogs.com/yunfeifei/p/4019504.html)
- [学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
- [JavaScript深入之闭包](https://github.com/mqyqingfeng/Blog/issues/9)

</details>

<details>
<summary>Object 常用方法</summary>

#### 参考

- [Object | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)

</details>

<details>
<summary>String 常用方法</summary>

#### 参考

- [String | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

</details>

<details>
<summary>Array 常用方法</summary>

#### 参考

- [Array | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

</details>

<details>
<summary>JS 实现千分位</summary>

- 正则

```js
function format (num) {  
  var reg=/\d{1,3}(?=(\d{3})+$)/g;   
  return (num + '').replace(reg, '$&,');  
}
```

- for循环

```js
function format(num){  
  num=num+'';//数字转字符串  
  var str="";//字符串累加  
  for(var i=num.length- 1,j=1;i>=0;i--,j++){  
      if(j%3==0 && i!=0){//每隔三位加逗号，过滤正好在第一个数字的情况  
          str+=num[i]+",";//加千分位逗号  
          continue;  
      }  
      str+=num[i];//倒着累加数字
  }  
  return str.split('').reverse().join("");//字符串=>数组=>反转=>字符串  
} 
```

#### 参考

- [JS 实现千分位](https://www.cnblogs.com/lvmylife/p/8287247.html)

</details>

<details>
<summary>深拷贝</summary>

```js
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                //判断ojb子元素是否为对象，如果是，递归复制
                if(obj[key]&&typeof obj[key] ==="object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}
```

#### 参考

- [JS 深拷贝](https://www.cnblogs.com/echolun/p/7889848.html)
- [JavaScript专题之深浅拷贝](https://github.com/mqyqingfeng/Blog/issues/32)

</details>

<details>
<summary>函数式编程</summary>

> 将复杂过程抽象成单一处理逻辑的纯函数编码思想，即一个函数只干一件事件，相同输入对应相同输出，不受外部环境影响，执行过程也不影响外部环境

#### 参考

- [漫谈 JS 函数式编程](http://web.jobbole.com/91602/)

</details>

<details>
<summary>import 和 require</summary>

- `require` 是 `AMD|CommonJS` 规范的实现，动态加载模块，在运行时确定模块的依赖关系及输入/输出的变量
- `import` 静态加载，在编译时期就确定输入/输出的变量

#### 参考

- [JS 中的require 和 import 区别](https://www.cnblogs.com/ariel-zhang/p/7127714.html)
- [Javascript(es2016) import和require用法和区别](https://blog.csdn.net/chinaycheng/article/details/52559439)
- [前端模块化（CommonJs,AMD和CMD）](https://www.jianshu.com/p/d67bc79976e6)

</details>

<details>
<summary>回调狱</summary>

- Promise
- async / await
- Generator
- * / yeild

#### 参考

- [JavaScript中避免回调地狱方法](https://blog.csdn.net/m0_37263637/article/details/80742239)

</details>

<details>
<summary>JS实现sleep</summary>

```js
function sleep(numberMillis) {
  var now = new Date();
  var exitTime = now.getTime() + numberMillis;
  while (true) {
    now = new Date();
    if (now.getTime() > exitTime)
      return;
  }
} 
```

#### 参考

- [javascript里模拟sleep(两种实现方式)](https://www.jb51.net/article/33581.htm)

</details>

<details>
<summary>值类型和引用类型</summary>

- 基本数据类型：`undefined`、`null`、`boolean`、`number`、`string`、`symbol`
- 引用数据类型：`object`、`array`、`function`

值类型直接指向值，引用类型指向内存地址

#### 参考

- [JS 的基本数据类型和引用数据类型](https://github.com/zanjs/awesome-frontend-interview/issues/6)

</details>

<details>
<summary>mul函数</summary>

- 递归 和 valueOf

```js
function mul(x) {
  const res = (y) => mul(x * y);
  res.valueOf = () => x;
  return res;
}
```

#### 参考

- [mul函数](https://www.cnblogs.com/newh5/p/6337038.html)

</details>

<details>
<summary>简单的模板引擎</summary>

主要思想通过 `new Function` 构造可执行的方法

```js
var fn = new Function("arg", "console.log(arg + 1);");
```

等同于

```js
var fn = function(arg) {
  console.log(arg + 1);
}
```

#### 参考

- [教你使用javascript简单写一个页面模板引擎](https://www.jb51.net/article/65480.htm)

</details>

<details>
<summary>JS 切割等长度的小数组</summary>

```js
const chunk = (arr, size) =>
Array.from({length: Math.ceil(arr.length / size)}, (v, i) => arr.slice(i * size, i * size + size));
// chunk([1,2,3,4,5], 2) -> [[1,2],[3,4],[5]]
```

#### 参考

- [30-seconds-of-code](https://github.com/kujian/30-seconds-of-code#chunk)

</details>

<details>
<summary>计算数组中值的出现次数</summary>

```js
const counts = (arr, value) => arr.reduce((a, v) => v === value ? a + 1 : a + 0, 0);
```

#### 参考

- [30-seconds-of-code](https://github.com/kujian/30-seconds-of-code#countoccurrences)

</details>

<details>
<summary>多层嵌套数组合并为一层</summary>

```js
const deepFlatten = arr => [].concat(...arr.map(v => Array.isArray(v) ? deepFlatten(v) : v));
```

#### 参考

- [30-seconds-of-code](https://github.com/kujian/30-seconds-of-code#deepFlatten)

</details>

<details>
<summary>数组去重</summary>

这种方式有意思哈

```js
const unique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i));
```

#### 参考

- [30-seconds-of-code](https://github.com/kujian/30-seconds-of-code#filternonunique)
- [JavaScript专题之数组去重](https://github.com/mqyqingfeng/Blog/issues/27)

</details>

<details>
<summary>从原型到原型链</summary>

`prototype` `__proto__` 是属性，并不是原型；`prototype` 是构造函数上的属性，而 `__proto__` 是实例对象上的属性; 而 构造的`prototype`属性指向的是一个对象，而这个对象才是原型，而实例对象的`__proto__`属性也是执行这个原型

即： `构造.prototype` === `原型` === `实例.__proto__`

而原型有一个 `constructor` 属性，这个属性指向的又是构造，因此又有了 

`原型.constructor` === `构造`,也有了下边的推导：

`原型.constructor` === `构造` === `构造.prototype.constructor` === `实例.__proto__.constructor`

#### 参考

- [JavaScript深入系列](https://github.com/mqyqingfeng/Blog/issues/2)

</details>

<details>
<summary>静态作用域和动态作用域</summary>

> 一个概念 静态作用域 即是 `词法作用域`

- JavaScript 采用的是`词法作用域`，函数的作用域在函数定义的时候就决定了
- 相对于静态的`动态作用域`，动态的作用域是在函数执行的时候决定的.

**变量提升**： 提升的是`声明`，不包含初始操作  
**函数提升**：同样提升的是 函数`声明`,而函数表达式不能提升的(`var fun = ()=>{}`; fun，虽然是个函数，但它(`fun`)是一个函数表达式，和普通的变量声明一样)，且 **函数提升优先级高于变量提升** ，如  

```js
f(); 
var scope = "local scope"; 
function f() { return ; } // 这里的scope是undefined

// 等价于

function f() {return scope};
var scope; // 变量提升，提升的只是申明
f(); // 执行的时候变量并没有赋值，所以是undefined
scope = "local scope";
```

#### 参考

- [JavaScript词法作用域和动态作用域](https://github.com/mqyqingfeng/Blog/issues/3)

</details>

<details>
<summary>执行上下文栈</summary>

> 管理JavaScript执行上下文的栈对象

JavaScript 的可执行代码(executable code)的类型：

- 全局代码
- 函数代码
- eval代码

栈底是全局上下文，只有当整个应用程序结束的时候，`执行上下文栈 EC-Stack`才会被清空

#### 参考

- [JavaScript深入之执行上下文栈](https://github.com/mqyqingfeng/Blog/issues/4)

</details>

<details>
<summary>变量对象</summary>

> 当 JavaScript 代码执行一段可执行代码(executable code)时，会创建对应的执行上下文(execution context)。对于每个执行上下文，都有三个重要属性：

- 变量对象(Variable object，VO)
- 作用域链(Scope chain)
- this

> 变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明

`变量对象` ：个人理解为存储了当前上下文对象下的 `变量`和 `函数声明` 的一个容器对象  

> 不同上下文下的变量对象是不同的，分为 `全局上下文下的变量对象` 和 `函数上下文下的变量对象`

### 全局上下文下的变量对象

> 全局上下文中的变量对象就是全局对象，如 浏览器中的 `window` 对象，Nodejs中的 `global`  
> 全局上下文下的变量对象使用 `VO` 表示  

### 函数上下文下的变量对象

> 函数上下文下中我们使用活动对象(activation object) `AO` 来表示变量对象

> **活动对象和变量对象其实是一个东西**，只是变量对象是规范上的或者说是引擎实现上的，不可在 JavaScript 环境中访问，只有到当进入 `一个执行上下文` 中，这个执行上下文的变量对象才会被激活，所以才叫 `activation object` ，而只有被激活的变量对象，也就是活动对象上的各种属性才能被访问

个人理解为：变量对象是一个抽象的概念或抽象实现，最开始（`声明的阶段`）是不能直接获取的；而只有在 `进入函数上下文`，`执行` 的时候（`解析的阶段`）才能被访问，也就是从 `VO`要变成 `AO`的话，需要 `进入函数上下文`

**执行上下文的代码会分成两个阶段进行处理：分析和执行，我们也可以叫做：**

- 进入执行上下文
- 代码执行

### 进入执行上下文

> 当进入执行上下文时，这时候还没有执行代码，

变量对象会包括：

1. 函数的所有形参 (如果是函数上下文)
    - 由名称和对应值组成的一个变量对象的属性被创建
    - 没有实参，属性值设为 undefined
2. 函数声明
    - 由名称和对应值（函数对象(function-object)）组成一个变量对象的属性被创建
    - 如果变量对象已经存在相同名称的属性，则完全替换这个属性
3. 变量声明
    - 由名称和对应值（undefined）组成一个变量对象的属性被创建；
    - 如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性

个人理解为：分为三个阶段:

> 函数上下文才会有这个阶段，会先进行函数参数的声明，即 `形参初始阶段`，这个阶段变量对象的`属性`的会被创建，而形参的名称作为 `key`，值 `value` 为调用时的值；这个阶段没有实参，其值全都是 `undefined`,如：

```js
AO = {
  arguments:{...},
  param: value,
  param: value,
}
```

> 第二个阶段是 `函数声明的阶段` ，在这个阶段中变量对象 `属性`被创建，而 `函数名` 会作为 `key`，其值是这个函数，如果存在同名函数的话，后边的会覆盖前边的函数声明，如：

```js
AO = {
  arguments:{...},
  fun: ()=>{},
  fun: ()=>{},
}
```

> 第三个阶段是 `变量声明的阶段` ，在这个阶段变量被声明，`变量名` 作为 `key`,其值全都是 `undefined`，如果变量名称跟 `已经声明的形式参数` 或 `函数相同`，那么这个变量声明直接跳过(忽略不执行)，因此 *变量声明不会干扰已经存在的这个属性*，如：

```js
AO = {
  arguments:{...},
  variable: undefined,
  variable: undefined,
}
```

### 代码执行阶段

> 在代码执行阶段，会顺序执行代码，根据代码，修改变量对象的值

```js
AO = {
  arguments:{...},
  param: value,
  param: value,
  fun: ()=>{},
  fun: ()=>{},
  variable: value,
  variable: value,
}
```

> 总结

- `全局上下文`的 `变量对象` 初始化是 `全局对象`
- `函数上下文` 的 `变量对象` 初始化 `只` 包括 `Arguments 对象`
- 在 `进入执行上下文` 时会给 `变量对象` 添加 `形参`、`函数声明`、`变量声明` 等初始的属性值
- 在 `代码执行` 阶段，会再次修改变量对象的属性值，`具体的代码执行操作`

注意：

> 注重注意的是`进入执行上下文`第二个阶段，这个阶段有了 `形参` `函数声明` 但 `变量声明` 只是声明，变量具体的值需要到代码执行的时候才能确定

如下代码：

```js
console.log(foo);

function foo(){
    console.log("foo");
}

var foo = 1;
```

等价于

```js
var foo = ()=>{} // 函数声明，函数提升
// var foo; 变量声明，变量提升，由于与函数同名，被忽略，因此不执行
console.log(foo); // 打印出来的值是函数

foo = 1; // foo 被重新赋值为变量的值
```

#### 参考

- [JavaScript深入之变量对象](https://github.com/mqyqingfeng/Blog/issues/5)

</details>

<details>
<summary>作用域链</summary>

> 当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链

个人理解为：作用域链就是保存了当前上下文，和所有父级(词法层面父级)上下文的一个栈集合，而这个上下文环境是使用 `VO` 对象保存，而在函数具体的执行阶段(执行代码的时候) ，由 `VO` 转化成 `AO`，而这个作用域链会在函数中使用一个叫 `Scope` 的属性定义，`Scope` 就是当前函数能访问的所有上下文的集合数组，因此函数能根据这个集合查找自己能访问的属性或变量

如原文例子：

```js
function foo() {
  function bar() {
    ...
  }
}
```

1. 第一步：`foo` 函数进入到函数声明，形参初始化，变量声明的阶段，这个时候呢，会创建 `VO`对象，并保存当前函数能访问的`VO`引用到上下文中的 `Scope`，最外层始终是有一个 `全局VO` 的，不然我们怎么能在函数内部访问到全局变量和函数呢，即

```js
// foo 的 VO 对象
VO = {
  arguments: {...},
  bar: undefined,
  this,
}
```

```js
ECStack = [
  fooContext:{
    Scope:[global.VO]
  },
  globalContext,
]
```

2. 函数 `foo` 执行的时候，会先做好准备工作（预编译吧），VO 变成 `AO`，并在这个阶段完成变量赋值等初始操作，并且 把当前函数的作用域保存到 上下文的作用域链 `Scope` 当中,即

```js
ECStack = [
  fooContext:{
    Scope:[foo.AO, global.VO]
  },
  globalContext:{
    Scope:[global.VO]
  },
]
```

至于为什么在函数执行阶段的准备阶段，才进行当前作用域链的拷贝工作，个人理解为在之前的阶段（声明阶段），函数自己都不知道能访问到哪些东西，因为申明阶段的所有变量都是 `undefined`,因此在 `完成准备阶段之后，执行代码之前`，保存当前上下文的引用到作用域链，那么接下来执行代码的时候就能够通过作用域链访问到所有定义过的属性或方法

3. 在 `foo` 进入上下文，foo内部函数声明阶段的时候，`bar` 函数被申明，那么`bar` 的 `AO` 被创建，同样的也会保留自己能访问到的所有父级上下文到自己上下文的 `Scope` ，即

```js
ECStack = [
  barContext:{
    Scope:[bar.AO, foo.VO, global.VO]
  },
  fooContext:{
    Scope:[foo.AO, global.VO]
  },
  globalContext:{
    Scope:[global.VO]
  },
]
```

有点比较重要的是 `只有当函数执行的时候才会进行压栈的操作`，上边的 `ECStack` 只是为了展示 `Scope` 保存的内容  

其次，在我们的 `闭包` 操作当中，内部函数(`bar`)在外部函数(`foo`)执行结束后，任能继续访问外部函数定义的变量，那也是因为 `内部函数` 的上下文中作用域链保存了外部函数的 `AO` 对象，即使 外部函数已经执行完毕，并外部上下文被销毁，但由于还保留着对外部 `AO` 的引用，内存中数据并没有销毁，因此也是能够访问的，那么如下闭包的例子也就能解释了

```js
function outter() {
  var param = 1;
  function inner() {
    console.log(param)
  }

  return inner;
}

outter()();
```

#### 参考

- [JavaScript深入之作用域链](https://github.com/mqyqingfeng/Blog/issues/6)
- [JavaScript深入之执行上下文](https://github.com/mqyqingfeng/Blog/issues/8)

</details>

<details>
<summary>从ECMAScript规范解读this</summary>

还理解不了😅

#### 参考

- [JavaScript深入之从ECMAScript规范解读this](https://github.com/mqyqingfeng/Blog/issues/7)

</details>

<details>
<summary>参数按值传递</summary>

函数参数传递的时候，参数数据类型分为

- 值类型
- 引用类型

但这只是参数的类型，真正在传递给函数的时候，值类型传递值的拷贝  

而引用类型参数传递 **引用的拷贝**，原文讨论中有一个形象的比喻就是，`文件`、`文件夹` 和 `快捷方式` ，那么值传递就是直接拷贝文件，而引用类型是拷贝的快捷方式，因此在函数内部 `直接` 修改值，就是修改了快捷方式，将引用地址改变而已，并没有修改引用地址所指向的具体指，然后在函数内部修改 `引用地址所指向的值`,那么相当于修改原来的文件

#### 参考

- [JavaScript深入之参数按值传递](https://github.com/mqyqingfeng/Blog/issues/10)

</details>

<details>
<summary>call和apply的模拟实现</summary>

思路为：

- 将函数赋值给要绑定的 `this` 对象的一个属性
- 执行这个属性指向的函数
- 删除该属性

```js
Function.prototype.call = function(context) {
  // 首先要获取调用call的函数，用this可以获取
  context.fn = this; // 将函数赋值给要绑定的 `this` 对象的一个属性
  context.fn(); // 执行这个属性指向的函数
  delete context.fn; // 删除该属性
}

var foo = {
  value: 1
};

function bar() {
  console.log(this.value);
}

bar.call(foo);
```

#### 参考

- [JavaScript深入之call和apply的模拟实现](https://github.com/mqyqingfeng/Blog/issues/11)

</details>

<details>
<summary>类数组对象与arguments</summary>

#### 参考

- [JavaScript深入之类数组对象与arguments](https://github.com/mqyqingfeng/Blog/issues/14)

</details>

<details>
<summary>创建对象的多种方式以及优缺点</summary>

4.1

```js
function Person(name) {
    this.name = name;
    if (typeof this.getName != "function") {
        Person.prototype = {
            constructor: Person,
            getName: function () {
                console.log(this.name);
            }
        }
    }
}

var person1 = new Person('kevin');
var person2 = new Person('daisy');

// 报错 并没有该方法
person1.getName();

// 注释掉上面的代码，这句是可以执行的。
person2.getName();
```

个人对于原文这个示例的理解：

> 讨论中有个示例图，解释的比较清楚的一点是：js创建一个对象时是 `先建立原型关系`，而 `后执行构造函数`  
那么在 `第一个` `var person1= new Person('Kevin')` 调用的时候，函数(类)的 `Person.prototype` 还并没有被修改，然后再执行类似 `Person.apply(obj)` 的操作，在这个apply操作中，构造被执行，那么 `if` 里边的内容被执行，然后 `Person.prototype` 才被修改，指向新的一个字面量对象，
重点是，这个时候 `person1` 的原型还是指向的被 `修改之前` 的 `Person.prototype`，而在第二次 `var person2 = new Person('Daisy')` 的时候，`Person.prototype` 已经被修改，因此 `person1` 原型上是没有 `getName`，而 `person2` 可以正常调用

#### 参考

- [JavaScript深入之创建对象的多种方式以及优缺点](https://github.com/mqyqingfeng/Blog/issues/15)

</details>

<details>
<summary>继承的多种方式和优缺点</summary>

对于原文示例 6 寄生组合式继承，及直接写 `Child.prototype = Parent.prototype;` 的方式的理解

```js
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.getName = function () {
    console.log(this.name)
}

function Child (name, age) {
    Parent.call(this, name);
    this.age = age;
}

Child.prototype = new Parent();

var child1 = new Child('kevin', '18');
```

```js
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.getName = function () {
    console.log(this.name)
}

function Child (name, age) {
    Parent.call(this, name);
    this.age = age;
}

// 关键的三步
var F = function () {};

F.prototype = Parent.prototype;

Child.prototype = new F();


var child1 = new Child('kevin', '18');
```

这关键的第三步，个人这样理解的

> 第一种方式 `Child.prototype = new Parent()`，Child的原型直接指向的是Parent的 `实例`，这种方式会调用两次 Parent 构造这一点毋庸置疑，有意思的是修改为

```js
// 关键的三步
var F = function () {};
F.prototype = Parent.prototype;
Child.prototype = new F();
```

```js
Child.prototype = Parent.prototype;
```

这两种方式，区别在于下边一种是将Child的原型直接指向了Parent的原型，因此在修改Child.prototype的时候，是会修改到Parent.prototype，因为这两个指向的是同一个对象(原型是一个对象)，而使用 `F` 中间函数的方式，我的理解为 Child的原型指向`F` 的实例，而实例 `new F()` 的原型才是指向 `Parent.prototype` ，因此如下图：

```
Child.prototype -> `new F()`: F实例 -- F实例.__proto__ --> Parent.prototype -> {}:Parent的原型
```

那么在修改 Child.prototype 的时候，其实是在 `实例F` 上修改而已，没有直接在Parent.prototype 上修改

可以理解为在 Child 和 Parent 之间添加了一个 `中间层` ，但是这并没有破坏原型的继承

#### 参考

- [JavaScript深入之继承的多种方式和优缺点](https://github.com/mqyqingfeng/Blog/issues/16)

</details>

<details>
<summary>防抖函数和节流函数</summary>

概念解释

- 函数防抖: 频繁触发,一段时间内没有重复触发，才会执行一次函数
- 函数节流: 频繁触发,一段时间内只执行一次函数

防抖原理：`clearTimeout & setTimeout` 的运用

```js
function debounce(func, wait) {
  var timeout;
  return function () {
    clearTimeout(timeout)
    timeout = setTimeout(func, wait);
  }
}
```

节流原理：函数 `执行标示` + `clearTimeout & setTimeout` 的运用

```js
function throttle(func, wait) {
  var timeout;
  var previous = 0;

  return function() {
    context = this;
    args = arguments;
    if (!timeout) { // 执行过后 timeout 是有值的，直到被赋值 null
      timeout = setTimeout(function(){
        timeout = null; // 关键操作
        func.apply(context, args)
      }, wait)
    }
  }
}
```

#### 参考

- [JavaScript专题之跟着underscore学防抖](https://github.com/mqyqingfeng/Blog/issues/22)
- [JavaScript专题之跟着underscore学节流](https://github.com/mqyqingfeng/Blog/issues/26)
- [js函数节流和函数防抖](https://www.cnblogs.com/fanfan-code/p/6400282.html)

</details>

