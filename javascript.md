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