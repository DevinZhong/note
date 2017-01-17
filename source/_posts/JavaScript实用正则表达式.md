---
title: JavaScript实用正则表达式
copyright: true
date: 2016-10-02 01:34:13
tags:
  - 正则表达式
  - JavaScript
---

## 匹配中文
```js
/**
 * 低配版，匹配所有非ASCII码字符
 */
/[^\u0000-\u00FF]/

/**
 * u4e00-u9fbf： unicode CJK(中日韩)统一表意字符。u9fa5后至u9fbf为空
 * uF900-uFAFF： 为unicode CJK 兼容象形文字。uFA2D后至uFAFF为空
 * 此正则式同时匹配中日韩文
 */
/[\u4E00-\u9FA5\uF900-\uFA2D]/
```

## 匹配全角字符
```js
/[\uFF00-\uFFEF]/
```