---
title: JavaScript基础
date: 2017-02-01 14:57:11
tags: JavaScript
categories: JavaScript
---

> JavaScript 基础都在这里了
<!-- more -->

## 对象使用和属性
- JavaScript 中所有变量都可以当作对象使用，除了两个例外 null 和 undefined
- 数字字面量不能直接当对象使用，因为解析器会将点操作符解析为浮点数字面值的一部分
- 中括号操作符中的属性名可以使用数值，也可以包含空格，而点操作符不行
```js
2.toString(); // 出错：SyntaxError
2..toString(); // 第二个点号可以正常解析
2 .toString(); // 注意点号前面的空格
(2).toString(); // 2先被计算

foo.1234; // SyntaxError
foo['1234']; // works
```

## 原型链
### 栗子
```js
function Foo() {
    this.value = 42;
}
Foo.prototype = {
    method: function() {}
};

function Bar() {}

// 设置Bar的prototype属性为Foo的实例对象
Bar.prototype = new Foo();
Bar.prototype.foo = 'Hello World';

// 修正Bar.prototype.constructor为Bar本身
Bar.prototype.constructor = Bar;

var test = new Bar() // 创建Bar的一个新实例

// 原型链
test [Bar的实例]
    Bar.prototype [Foo的实例] 
        { foo: 'Hello World',
          value: 42 }
        Foo.prototype
            {method: ...};
            Object.prototype
                {toString: ... /* etc. */};
```
### 总结
- 在写复杂的 JavaScript 应用时，要提防原型链过长带来的性能问题，并知道如何通过缩短原型链来提高性能。
- 绝对不要扩展内置类型的原型，除非是为了和新的 JavaScript 引擎兼容。

## hasOwnProperty 函数
- hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数。

## 命名函数的赋值表达式
```js
var foo = function bar() {
    bar(); // 正常运行
}
bar(); // 出错：ReferenceError
```
bar 函数声明外是不可见的，这是因为我们已经把函数赋值给了 foo； 然而在 bar 内部依然可见。这是由于 JavaScript 的命名处理所致，函数名在函数内总是可见的。

## this
- 在严格模式下（strict mode），全局作用域的 this 将会是 undefined。

### 方法的赋值表达式
```js
var test = someObject.methodTest;
test(); // 此时函数内部的 this 并不指向 someObject
```

## 闭包
### 匿名包装器（自执行匿名函数）
```js
for(var i = 0; i < 10; i++) {
    (function(e) {
        setTimeout(function() {
            console.log(e);  
        }, 1000);
    })(i);
}
// or
for(var i = 0; i < 10; i++) {
    setTimeout((function(e) {
        return function() {
            console.log(e);
        }
    })(i), 1000)
}
```

## arguments 对象
- 尽管在语法上它有数组相关的属性 length，但它不从 Array.prototype 继承，实际上它是一个对象
- 由于 arguments 已经被定义为函数内的一个变量。因此通过 var 关键字定义 arguments 或者将 arguments 声明为一个形式参数，都将导致原生的 arguments 不会被创建（被覆盖）
- `Array.prototype.slice.call(arguments)` 可将 arguments 转化为数组，不过速度较慢
- 严格模式下 arguments 的属性并不会创建成 getters 和 setters 与传入参数绑定，即修改后互不影响

### arguments 传递技巧
```js
// 下面是将参数从一个函数传递到另一个函数的推荐做法
function foo() {
  // 相当于在全局作用域内以此函数的参数为参数调用 bar
  bar.apply(null, arguments);
}
function bar(a, b, c) {
  // 干活
}


// 也可以同时使用 call 和 apply，为函数创建一个快速的解绑定包装器
function Foo() {}
Foo.prototype.method = function(a, b, c) {
    console.log(this, a, b, c);
};
// 此函数即为 Foo.prototype.method 的解绑定函数
// 输入参数为: this, arg1, arg2...argN
Foo.method = function() {
    // 结果: Foo.prototype.method.call(this, arg1, arg2... argN)
    Function.call.apply(Foo.prototype.method, arguments);
};
// 其实就是这样
Foo.method = function() {
    var args = Array.prototype.slice.call(arguments);
    Foo.prototype.method.apply(args[0], args.slice(1));
};
```

## 通过工厂模式创建对象
- 通过闭包，这种方式能创建真正的私有变量
- 抛弃原型链，占用更多内存，与语言本身思想相悖

```js
function Foo() {
    var obj = {};
    obj.public = 'public value here';
    var private = 'private value here';

    obj.setPrivate = function(value) {
        private = value;
    }
    obj.getPrivate = function() {
        return private;
    }

    return obj;
}
```

## 函数变量名解析顺序
当访问函数内的 foo 变量时，JavaScript 会按照下面顺序查找：
1. 当前作用域内是否有 var foo 的定义
2. 函数形式参数是否有使用 foo 名称的
3. 函数自身是否叫做 foo
4. 回溯到上一级作用域，然后从 #1 重新开始


## 变量
- 如果重新声明 JavaScript 变量，该变量的值不会丢失
- 值 undefined 实际上是从值 null 派生来的，因此 ECMAScript 把它们定义为相等的
- 未声明的变量无法用于表达式，会报 ReferenceError

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
