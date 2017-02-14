---
title: DOM
copyright: true
date: 2016-10-02 01:24:28
tags:
  - DOM
  - HTML
  - JavaScript
---

> 文档对象模型 (DOM) 是 HTML 和 XML 文档的编程接口。它给文档（结构树）提供了一个结构化的表述并且定义了一种方式—程序可以对结构树进行访问，以改变文档的结构，样式和内容。 DOM 提供了一种表述形式— 将文档作为一个结构化的节点组以及包含属性和方法的对象。从本质上说，它将 web 页面和脚本或编程语言连接起来了。
<!-- more -->

## 事件
### 判断是否按下Ctrl
```js
action(e) {
  if (e.ctrlKey) {
    // 按下了
  } else {
    // 没按
  }
}
```

### 捕获两个按键
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

### a标签不跳转的一种写法（不知有什么优势）
```html
<a href="javascript:;"></a>
```

### 监听页面关闭
```js
window.addEventListener("beforeunload", function( event ) {
    event.returnValue = "放弃当前未保存内容而关闭页面？";
});

// 监听卸载事件
window.onunload = () => {
  alert("您确定离开该网页吗？");
}
```

### 模拟触发内置事件
```js
function simulateClick() {
  var event = new MouseEvent('click', {
    'view': window,
    'bubbles': true,
    'cancelable': true
  });
  var cb = document.getElementById('checkbox'); 
  var canceled = !cb.dispatchEvent(event);
  if (canceled) {
    // A handler called preventDefault.
    alert("canceled");
  } else {
    // None of the handlers called preventDefault.
    alert("not canceled");
  }
}
```

## 元素位置及尺寸

### 尺寸
- clientWidth = width + padding
- offsetWidth = clientWidth + border + 滚动条
- scrollWidth = clientWidth + [overflowWidth]


### 位置
- offsetParent：只读属性，返回一个指向最近的（closest，指包含层级上的最近）包含该元素的定位元素。如果没有定位的元素，则 offsetParent 为最近的 table 元素对象或根元素（标准模式下为 html；quirks 模式下为 body）。当元素的 style.display 设置为 "none" 时，offsetParent 返回 null。

- offsetTop = 只读属性，它返回当前元素相对于其 offsetParent 元素的顶部的距离
- offsetWidth = 只读属性，内边距的边缘与边框的外边缘之间的距离，相当于边框厚度（如果滚动条存在则加上）
- scrollTop = 滚动距离，可写


### 其他概念
视口：显示文档内容的浏览器的一部分，也就是当前窗口显示页面部分，不包括滚动条
- window.innerHeight：浏览器窗口的内部高度（浏览器的视口，不包括工具栏和滚动条） 
- window.innerWidth：浏览器窗口的内部宽度（浏览器的视口，不包括工具栏和滚动条）


### 获得视口坐标
ele.getBoundingClientRect()