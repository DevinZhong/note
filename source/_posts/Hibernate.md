---
title: Hibernate
date: 2017-02-04 22:45:30
tags:
  - Java
  - Hibernate
categories: Java
---

> Hibernate是一種Java語言下的物件關係對映解決方案。它是使用GNU寬通用公共許可證發行的自由、開源的軟體。它為物件導向的領域模型到傳統的關係型資料庫的對映，提供了一個使用方便的框架。
<!-- more -->

## 代码示例
```java
/**
 * 声明三个属性
 */
private SessionFactory sessionFactory;
private Session session;
private Transaction transaction;

/**
 * 开启方法
 */
// 注册服务
StandardServiceRegistry registry = new StandardServiceRegistryBuilder().configure().build();
// 创建会话工厂
sessionFactory = new MetadataSources(registry).buildMetadata().buildSessionFactory();
// 创建会话
session = sessionFactory.getCurrentSession();
// 开启事务
transaction = session.beginTransaction();

/**
 * 关闭方法
 */
transaction.commit(); // 提交事物
session.close(); // 关闭会话工厂
sessionFactory.close(); // 关闭会话工厂
```

## 标识符生成器
标识符生成器 | 描述
--------------|------------------------------------------------
increment | 适用于代理主键。由 Hibernate 自动以递增方式生成
identity | 适用于代理主键。由底层数据库生成标识符
sequence | 适用于代理主键。Hibernate 根据底层数据库的序列生成标识符，这要求底层数据库支持序列
hilo | 适用于代理主键。Hibernate 分局 high/low 算法生成标识符
seqhilo | 适用于代理主键。使用一个高/低位算法来高效地生成 long,short 或者 int 类型的标识符
native | 适用于代理主键。根据底层数据库对自动生成标识符的方式，自动选择 identity、sequence 或 hilo
uuid.hex | 适用于代理主键。Hibernate 采用 128 位的 UUID 算法生成标识符
uuid.string | 适用于代理主键。UUID 被编码成一个 16 字符长的字符串
assigned | 适用于自然主键。由 Java 应用程序负责生成标识符
foreign | 适用于代理主键。使用另外一个相关联的对象的标识符


## 处理二进制文件
### 将照片存入数据库
```java
// 先获得照片文件
File f = new File("d:" + File.separator + "boy.jpg");
// 获得照片文件的输入流
InputStream input = new FileInputStream(f);
// 创建一个 Blob 对象
Blob image = Hibernate.getLobCreator(session).createBlob(input, input.available());
// 设置照片属性
s.setPicture(image);
```

### 将照片读出
```java
Students s = (Students)session.get(Students.class, 1);
// 获得 Blob 对象
Blob image = s.getPicture();
// 获得照片的输入流
InputStream input = image.getBinaryStream();
// 创建输出流
File f = new File("d:" + File.separator + "dest.jpg");
// 获得输出流
OutputStream output = new FileOutputStream(f);
// 创建缓冲区
byte[] buff = new byte[input.available()];
input.read(buff);
output.write(buff);
input.close();
output.close();
```