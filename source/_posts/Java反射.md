---
title: Java反射
date: 2017-02-04 23:00:44
tags: Java
categories: Java
---

> Java 提供的反射機制允許您於執行時期動態載入類別、檢視類別資訊、生成物件或操作生成的物件，要舉反射機制的一個應用實例，就是在整合式開發環境中所提供的方法提示或是類別檢視工具，另外像 JSP 中的 JavaBean 自動收集請求資訊也使用到反射，而一些軟體開發框架（Framework）也常見到反射機制的使用，以達到動態載入使用者自訂類別的目的。
<!-- more -->

## 三观俱毁
- 类是对象，类是 java.lang.Class 类的实例对象
- 成员变量也是对象，是 java.lang.reflect.Field 类的实例对象
- 构造函数也是对象，是 java.lang.Constructor 类的实例对象

```java
Class c1 = int.class;
Class c2 = String.class;
Class c3 = double.class;
Class c4 = Double.class;
Class c5 = void.class;
```

## 获取类的类的 class 对象的方式
```java
// 任何一个类都有一个隐含的静态成员变量 class
Class c1 = Foo.class;

// 通过类的实例对象的 getClass 方法获得
Class c2 = foo.getClass();
```

## 反射的优势
- new 创建对象是静态加载类，在编译时就需要加载所有的可能使用到的类
- 通过反射则可以在运行时动态加载类


## Class 的实例方法
```java
// 获取所有 public 的成员变量的信息，返回一个 Field[]
c.getFields();
// 获取该类自己声明的成员变量的信息
c.getDeclaredFields();
// 获取所有的 public 的构造函数
c.getConstructors();
// 获取所有的构造函数
c.getDeclaredConstructors();
```

## 再说泛型
- 编译之后集合的泛型是去泛型化的
- Java 中集合的泛型，是防止错误输入的，只在编译阶段有效，绕过编译就无效了