---
title: D3.js
date: 2017-02-07 18:51:22
tags:
  - D3.js
  - JavaScript
categories: JavaScript
---

> D3.js is a JavaScript library for manipulating documents based on data. D3 helps you bring data to life using HTML, SVG, and CSS.
<!-- more -->


```js
// 有数据，而没有足够图形元素的时候，使用此方法可以添加足够的元素。
svg.selectAll("rect")   //选择svg内所有的矩形
    .data(dataset)  //绑定数组
    .enter()        //指定选择集的enter部分
    .append("rect") //添加足够数量的矩形元素
```

```js
d3.max()
d3.min()
d3.scale.linear()
d3.scale.ordinal()


svg.append("g").call(axis);
与
axis(svg.append(g));
是相等的。
```