---
title: Java框架零细
copyright: true
date: 2016-09-28 15:04:15
tags: Java
---

## 待整理
- `@RequestMapping(produces={"application/json;charset=UTF-8"})` 返回JSON数据时注意加这个属性
- 系统允许异常不必打日志，内部未知异常需要日志
- SpringMVC的几个关键（基本）组件：DispatcherServlet, DefaultAnnotationHandlerMapping, DefaultAnnotationHandlerAdapter, InternalResourceViewResolver
- row_count()上一条修改类型sql的影响行数


## redis 的启动
```bash
# 启动redis服务
redis-server
# 启动客户端
redis-cli -p 6379
# 使用默认端口6379
redis-cli
```

## redis 命令
```
# redis信息
info
# 缓存数据数量
dbsize
# 查看keys，比如：“seckill:1001”
keys *
# 拿到序列化串
get seckill:1001
```

## 引入依赖(Jedis和protostuff)
```xml
<!-- redis客户端：Jedis -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.7.3</version>
</dependency>
<!-- protostuff序列化依赖 -->
<dependency>
    <groupId>com.dyuproject.protostuff</groupId>
    <artifactId>protostuff-core</artifactId>
    <version>1.0.8</version>
</dependency>
<dependency>
    <groupId>com.dyuproject.protostuff</groupId>
    <artifactId>protostuff-runtime</artifactId>
    <version>1.0.8</version>
</dependency>
```