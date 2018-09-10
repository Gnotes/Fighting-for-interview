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

</details>

<details>
<summary>不规则图形事件处理</summary>

**热区处理**

</details>

<details>
<summary>闭包</summary>

> 一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分

#### 参考

- [全面理解Javascript闭包和闭包的几种写法及用途](https://www.cnblogs.com/yunfeifei/p/4019504.html)
- [学习Javascript闭包（Closure）](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)

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

</details>

<details>
<summary>函数式编程</summary>

> 将复杂过程抽象成单一处理逻辑的纯函数编码思想，即一个函数只干一件事件，相同输入对应相同输出，不受外部环境影响，执行过程也不影响外部环境

#### 参考

- [漫谈 JS 函数式编程](http://web.jobbole.com/91602/)

</details>

<details>
<summary>import 和 require</summary>

- `require` 是 `AMD|CommonJS` 规范的实现，动态加载模块，在运行时确定模块的依赖关系及输入/输出的变量
- `import` 静态加载，在编译时期就确定输入/输出的变量

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