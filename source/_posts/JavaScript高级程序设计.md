---
title: JavaScript高级程序设计
date: 2017-02-04 21:46:49
tags:
  - JavaScript
categories: JavaScript
---

> 错误整理和总结
<!-- more -->

## 错误整理
页码 | 错误点 | 修正
------|-------------------------|----------------------------------------
22 | Identifier Expected | Identifier Unexpected
35 | isPrototypeOf(object): 用于检查传入的对象是否是当前对象的原型 | 反了
39 | 在将一元减操作符应用于数值时，该值会变成负数 | “负数”说法不当，这是“取反”的逻辑
72 | 变量 person 是 Object 吗？ | 表述不当，应为“是 Object 的实例吗？”
79 | 循环引用指的是对象 A 中包含一个指向对象 B 的指针 ，而对象 B 中也包含一个 指向对象 A 的引用 | 一个意思用“指针”和“引用”两种表述，容易让读者纠结困惑
93 | reverse() 和 sort() 方法的返回值是经过排序之后的数组 | reverse() 并不进行排序
103 | 匹配字符串中所有"at"的实例 | “实例”一词或有不妥
104 | 匹配第一个" [bc]at"，不区分大小写 | 多了一个空格