---
title: DOM
copyright: true
date: 2016-10-02 01:24:28
tags:
  - DOM
  - HTML
  - JavaScript
---

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

### a标签不跳转的一种写法（不知有什么优势）
```html
<a href="javascript:;"></a>
```

### 监听页面关闭
```js
window.addEventListener("beforeunload", function( event ) {
    event.returnValue = "放弃当前未保存内容而关闭页面？";
});
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
offsetWidth = width + padding + border
clientWidth = width + padding
scrollWidth = width + padding + [overflowWidth]


### 位置
offsetTop = 元素左上角相对于以定位父容器左上角距离
clientWidth = 内边距的边缘与边框的外边缘之间的距离，相当于边框厚度
scrollTop = 滚动距离，可写


### 其他概念
视口：显示文档内容的浏览器的一部分，也就是当前窗口显示页面部分，不包括滚动条

### 获得视口坐标
ele.getBoundingClientRect()