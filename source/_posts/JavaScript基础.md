---
title: JavaScript基础
date: 2017-02-01 14:57:11
tags: JavaScript
categories: JavaScript
---

> JavaScript 基础都在这里了
<!-- more -->

## 变量
- 如果重新声明 JavaScript 变量，该变量的值不会丢失
- 值 undefined 实际上是从值 null 派生来的，因此 ECMAScript 把它们定义为相等的

```js
typeof null
// "object"
typeof Function
// "function"

null == undefined  // true
NaN == NaN  // false

let num = -0  // It's OK.

NaN < 3  // false
NaN >= 3  // false

true == 1  // true
true == 2  // false
undefined == 0  // false
null == 0  // false
null == undefined  // true
null === undefined  // false

var num = (5, 1, 4, 8, 0);  // num 的值为 0
```

```js
var message;
alert(message);  // "undefined"
alert(age);      // Error occurred
```

```js
var message;
alert(typeof message);  // "undefined"
alert(typeof age);      // "undefined"
```

## 引用类型
```js
// 模拟 push
arr[arr.length] = 'new value'
arr[arr.length] = 'another value'
```

## JSON
```js
var obj = {val:undefined,a:NaN,b:Infinity,c:new Date()}
JSON.stringify(obj)
// "{"a":null,"b":null,"c":"2017-02-01T07:16:28.709Z"}"
```

```js
var obj = {
  x: 1,
  y: 2,
  o: {
    o1: 1,
    o2: 2,
    toJSON() {
      return this.o1 + this.o2
    }
  }
}
// "{"x":1,"y":2,"o":3}"
JSON.stringify(obj)
```

## prototype

```js
Student.prototype = Object.create(Person.prototype)
Student.prototype.constructor = Student
```

必须使用new创建对象：

```js
if (!this instanceof DetectorBase) {
  throw new Error('Do not invoke without new.')
}
```

继承：

```js
function inherit(subClass, superClass) {
  subClass.prototype = Object.create(superClass.prototype)
  subClass.prototype.constructor = subClass
}
```

```js
// export to global object
Object.defineProperties(global, {
  LinkDetector: {value: LinkDetector},
  ContainerDetector: {value: ContainerDetector},
  DetectorBase: {value: DetectorBase}
})
```

## 正则表达式
| xx | 说明 | 示例 |
|-----|----------------------|---------------|
| `\n` | 表示使用分组符（x）匹配到的字符串 | `/(abc)\1/.test('abcabc')` |
| `(?:x)` | 仅分组 | `/(?:abc)(def)\1/.test('abcdefdef')` |

```js
/abc/g.global  // true
/abc/g.ignoreCase  // false
/abc/g.multiline  // false
/abc/g.source  // 'abc'
```

## 捕获两个按键
把按键状态缓存下来，类似这样：

```js
var pressingStatus = {};

elem.onkeydown = function (e) {
pressingStatus[e.keyCode] = true;

if (pressingStatus[keyA] && pressingStatus[keyB] && ...) {
// do something
}
};

elem.onkeyup = function (e) {
pressingStatus[e.keyCode] = false;
}
```
