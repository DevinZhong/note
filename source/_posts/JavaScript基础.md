---
title: JavaScript基础
date: 2017-02-01 14:57:11
tags: JavaScript
categories: JavaScript
---

> JavaScript 基础都在这里了
<!-- more -->


## 变量
- ~~如果重新声明 JavaScript 变量，该变量的值不会丢失。~~其实是声明提升所致
- 值 undefined 实际上是从值 null 派生来的，因此 ECMAScript 把它们定义为相等的
- 未声明的变量无法用于表达式，也无法用于参数传递，会报 ReferenceError（可以使用 typeof）

```js
var message;
// 未声明的变量无法用作参数
console.log(message);  // "undefined"
console.log(age);      // ReferenceError
// 但还能用 typeof
console.log(typeof message);  // "undefined"
console.log(typeof age);      // "undefined"
```


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

### ES5 的继承
```js
function inherit(subClass, superClass) {
  subClass.prototype = Object.create(superClass.prototype)
  subClass.prototype.constructor = subClass
}
```

### hasOwnProperty 函数
- hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数


### 总结
- 在写复杂的 JavaScript 应用时，要提防原型链过长带来的性能问题，并知道如何通过缩短原型链来提高性能。
- 绝对不要扩展内置类型的原型，除非是为了和新的 JavaScript 引擎兼容。


## 函数
### 命名函数的赋值表达式
```js
// bar 函数声明外是不可见的，这是因为我们已经把函数赋值给了 foo
// 然而在 bar 内部依然可见，这是由于 JavaScript 的命名处理所致，函数名在函数内总是可见的
var foo = function bar() {
    bar(); // 正常运行
}
bar(); // 出错：ReferenceError
```

### this
- 在严格模式下（strict mode），全局作用域的 this 将会是 undefined
- 要注意方法的赋值表达式
- 可以用 bind 方法对 this 进行绑定

```js
var test = someObject.methodTest;
test(); // 此时函数内部的 this 并不指向 someObject

// 对 this 进行绑定的经典例子
var queryAll = document.querySelectorAll.bind(document);
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


## 解决命名空间冲突
```js
(function() {
    // 函数创建一个命名空间
    var bar;  // 需要的资源自给自足，内部使用
    ...

    window.foo = function() {
        // 对外公开的函数，创建了闭包
    };

})(); // 立即执行此匿名函数
```

### 自执行匿名函数表达式
```js
( // 小括号内的函数首先被执行
function() {}
) // 并且返回函数对象
() // 调用上面的执行结果，也就是函数对象

// 另外两种方式
+function(){}();
(function(){}());
```

## 数组
巧妙运用构造函数拼接循环字符串
```js
// new Array(3).join('#') 将会返回 ##
new Array(count + 1).join(stringToRepeat);
```

## 等于操作符
- 字符串与数值比较时，字符串被强转为数值
- 布尔值与字符串比较时，布尔值被强转为字符串，"0" 与 "1"
- 布尔值与数值比较时，布尔值被强转为数值，true 为 1，false 为 0
- null == undefined，然后这两基佬与其它值都不等，比如不等于 false

### 当等于操作符遭遇对象
```js
{} === {} // false
{} == {}  // false

// 以下可见等于操作符并不好理解，在对象的处理上它并不完全类似于指针
// 他会取 toString() 的返回值进行比较
new String('foo') === 'foo'; // false
new Number(10) === 10;       // false
new String('foo') == 'foo';  // true
new Number(10) == 10;        // true，经过了对象到字符串，再到数值的转换
```

## 类型检测
### typeof 操作符
- 值的种类："symbol"、"undefined"、"string"、"number"、"boolean"、"object"、"function"
- 利用 typeof 的逻辑缺陷，可以用它区分函数和对象，在它看来函数是特殊的对象
- typeof(obj) 并不是一个函数调用，这对小括号只是用来计算一个表达式的值，这个返回值会作为 typeof 操作符的一个操作数，实际上不存在名为 typeof 的函数
- 测试变量是否未定义（指未赋值），是 typeof 真正实用的场合

```js
if (typeof foo === 'undefined') {
    console.log('未定义')
}
```

### 内部属性 [[Class]]
- [[Class]] 的值只可能是下面字符串中的一个： Arguments, Array, Boolean, Date, Error, Function, JSON, Math, Number, Object, RegExp, String, Undefined（JavaScript 1.8.5 之后）, Null（JavaScript 1.8.5 之后）

```js
// 用于比较
function is(type, obj) {
    // 比如对 "[object Array]" 进行切割
    var clas = Object.prototype.toString.call(obj).slice(8, -1);
    return clas === type;
}

is('String', 'test'); // true
is('String', new String('test')); // true
is('Null', null);  // true
```

### instanceof 比较内置类型纯粹作死
```js
new String('foo') instanceof String; // true
new String('foo') instanceof Object; // true

'foo' instanceof String; // false
'foo' instanceof Object; // false
```

## 类型转换
```js
isNaN(null)  // false，isNaN 函数会将 null 转换为 0

// 内置类型（比如 Number 和 String）的构造函数在被调用时，
// 使用或者不使用 new 的结果完全不同
new Number(10) === 10;     // False, 对象与数字的比较
Number(10) === 10;         // True, 数字与数字的比较
new Number(10) + 0 === 10; // True, 由于隐式的类型转换

// 数值与字符串之间的转换技巧
'' + 10 === '10'  // true
+'10' === 10      // true

// 字符串转换为数值的常用方法
+'010' === 10
Number('010') === 10
parseInt('010.2', 10) === 10  // 用来转换为整数

// “非”操作符使用两次，可将值转换为布尔型
!!'';      // false
!!'0';     // true
!!{};      // true
!!true;    // true
```

## 神奇的 undefined
- undefined 是一个值为 undefined 的全局变量，而它的值可被覆盖
- 在 ECMAScript 5 的严格模式下，undefined 不再是 可写的了，但是它的名称仍然可以被隐藏，比如定义一个函数名为 undefined
- 使用匿名包装器可避免全局 undefined 的值被覆盖带来的麻烦

```js
var undefined = 123;
(function(something, foo, undefined) {
    // 局部作用域里的 undefined 变量重新获得了 `undefined` 值
})('Hello World', 42);

// or
var undefined = 123;
(function(something, foo) {
    var undefined;
    ...
})('Hello World', 42);
```

## 自动分号插入
- 在前置括号的情况下，解析器不会自动插入分号，括号内容作为前面的参数
- return 对象时，做括号另起一行将会自动插入分号，返回 undefined


## setTimeout 和 setInterval
- 定时处理不是 ECMAScript 的标准，它们在 DOM (文档对象模型) 被实现
- 作为第一个参数的函数将会在全局作用域中执行，因此函数内的 this 将会指向这个全局对象
- 应该避免使用 setInterval，因为它的定时执行不会被 JavaScript 阻塞，使用 setTimeout 会更容易控制
- 建议不要在调用定时器函数时，为了向回调函数传递参数而使用字符串的形式

```js
// 第一个参数的函数将在全局作用域中执行
function Foo() {
    this.value = 42;
    this.method = function() {
        // this 指向全局对象
        console.log(this.value); // 输出：undefined
    };
    setTimeout(this.method, 500);
}
new Foo();
```

```js
// 使用 setTimeout 代替 setInterval 会更容易控制
function foo(){
    // 阻塞执行 1 秒
    setTimeout(foo, 100);
}
foo();
```

```js
// 建议不要在调用定时器函数时，为了向回调函数传递参数而使用字符串的形式
function foo(a, b, c) {}

// 不要这样做
setTimeout('foo(1, 2, 3)', 1000)

// 可以使用匿名函数完成相同功能
setTimeout(function() {
    foo(1, 2, 3);
}, 1000)
```

## JSON
### JS 对象转化为 JSON 时空值的处理
```js
var obj = {val:undefined,a:NaN,b:Infinity,c:new Date()}
JSON.stringify(obj)
// "{"a":null,"b":null,"c":"2017-02-01T07:16:28.709Z"}"
```

### toJSON 函数的使用
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
JSON.stringify(obj)
// "{"x":1,"y":2,"o":3}"
```

## Object.defineProperties(obj, props)
```js
var obj = {};
Object.defineProperties(obj, {
  "property1": {
    value: true,
    writable: true
  },
  "property2": {
    value: "Hello",
    writable: false
  }
  // 等等.
});
alert(obj.property2) //弹出"Hello"
```

## 正则表达式
xx | 说明 | 示例
-----|----------------------|--------------
`\n` | 表示使用分组符（x）匹配到的字符串 | `/(abc)\1/.test('abcabc')`
`(?:x)` | 仅分组 | `/(?:abc)(def)\1/.test('abcdefdef')`

```js
/abc/g.global  // true
/abc/g.ignoreCase  // false
/abc/g.multiline  // false
/abc/g.source  // 'abc'
```
