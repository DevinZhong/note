---
title: Java注解
date: 2017-02-04 23:37:24
tags: Java
categories: Java
---

> 注解使得Java源代码中不但可以包含功能性的实现代码，还可以添加元数据。 注解的功能类似于代码中的注释，所不同的是注解不是提供代码功能的说明，而是实现程序功能的重要组成部分。
<!-- more -->

## 注解示例
```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Description { // 使用 @interface 关键字定义注解
  // 成员类型是受限的，合法的类型包括原始类型及 String,Class,Annotation,Enumeration
  // 如果注解只有一个成员，则成员名必须取名为 value()，在使用时可以忽略成员名和赋值号（=）
  // 注解类可以没有成员，没有成员的注解称为标识注解
  String desc(); // 成员以无参无异常方式声明
  String author();
  int age() default 18; // 可以用 default 为成员指定一个默认值
}
```

## 元注解
```java
// CONSTRUCTOR（构造方法声明）,FIELD（字段声明）,LOCAL_VARIABLE（局部变量声明）,
// METHOD（方法声明）,PACKAGE（包声明）,PARAMETER（参数声明）,TYPE（类接口）
@Target({ElementType.METHOD, ElementType.TYPE})

// SOURCE 只在源码显示，编译时会丢弃
// CLASS 编译时会记录到 class 中，运行时忽略
// RUNTIME 运行时存在，可以通过反射读取
@Retention(RetentionPolicy.RUNTIME)
```