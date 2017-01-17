---
title: jQuery-free
copyright: true
date: 2016-10-02 01:30:36
tags: JavaScript
---

## 事件监听
```js
// dom ready
document.addEventListener('DOMContentLoaded',function(){});

// dom listen
element.addEventListener('evnt', function() {}, false);

// get dom parent
element.parentNode
```

## 查询
```js
// query by id
document.querySelector('#myElement');

// query by css class
document.querySelectorAll('.myElement');

// query by tag name
documnet.querySelectorAll('div');

// query by attribute
document.querySelectorAll('a[target=_blank]');

// 上一元素
element.previousElementSibling
// 下一元素
element.nextElementSibling

// 兄弟节点
[].filter.call(el.parentNode.children, function(child) {
  return child !== el;
});
```

## 元素属性及样式
```js
// get/set attributes
element.getAttribute('data-test');
element.setAttribute('data-test', 'true');

// get style
win.getComputedStyle(el, null).color;

// add/remove css class
element.classList.add('xxx');
element.classList.remove('xxx');
element.className += 'xxx';
element.className = element.className.replace(/^bold$/, '');

// toggle class
element.classList.toggle(classname)

// change css style
element.style.display('none');


// hasClass
element.classList.contains('xxx');
element.className.match(/xxx/);
```

## 元素内容
```js
// get text
el.textContent
// set text
el.textContent = string;

// get html
el.innerHTML
// set html
el.innerHTML = htmlString;
```

## 添加删除元素
```js
// dom prepend/append
element.insertAdjacentHTML('afterbegin','something u want to insert');
element.insertAdjacentHTML('beforeend','something u want to insert');



// remove
el.parentNode.removeChild(el);
```

## 宽高
```js
// window height(不含 scrollbar)
window.document.documentElement.clientHeight;
// window height(含 scrollbar)
window.innerHeight

// document height
document.documentElement.scrollHeight

// element height
// 精确到整数（border-box 时为 height 值，content-box 时为 height + padding + border 值）
el.clientHeight;
// 精确到小数（border-box 时为 height 值，content-box 时为 height + padding + border 值）
el.getBoundingClientRect().height;

// iframe height
iframe.contentDocument.documentElement.scrollHeight;
```

## 定位
```js
// position
{ left: el.offsetLeft, top: el.offsetTop }

// offset
function getOffset (el) {
  const box = el.getBoundingClientRect();

  return {
    top: box.top + window.pageYOffset - document.documentElement.clientTop,
    left: box.left + window.pageXOffset - document.documentElement.clientLeft
  }
}
```