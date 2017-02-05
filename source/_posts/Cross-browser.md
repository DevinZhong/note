---
title: Cross-browser
date: 2017-02-04 21:34:57
tags:
  - Cross-browser
---

> 跨浏览器技术有点算黑技术，不过这是我们征服浏览器兼容性问题路上的必修课
<!-- more -->

## IE 条件注释
[参考来源](http://blog.chenxu.me/post/detail?id=3ebbe61d-4a4a-46d4-8ba7-233192c1f92b)
```html
<!-- 下面这段是 向下隐藏 语法-->
<!--[if lt IE 9]>
  <script src="~/Scripts/jquery-1.9.1.min.js"></script>    
<![endif]-->

<!-- 下面这段是 向下显示 语法--> 
<!--[if gte IE 9]><!-->    
  <script src="~/Scripts/jquery-2.0.0.min.js"></script>    
<!--<![endif]-->
```
