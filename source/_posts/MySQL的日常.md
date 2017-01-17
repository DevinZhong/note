---
title: MySQL的日常
copyright: true
date: 2016-09-27 01:48:06
tags: MySQL
---

## 新建数据库时指定编码和排序
```mysql
CREATE DATABASE `sun` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

## 事务的优点
- 原子性
- 隔离性
- 一致性
- 持久性