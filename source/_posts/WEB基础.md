---
title: WEB基础
copyright: true
date: 2016-09-27 22:45:06
tags: WEB
---

## RESTful
资源不能是动词，但是可以是一种服务。比如转账不要用transfer动词，而要用transaction表示转账服务。
POST /transaction?from=1&to=2&amount=500.00


## 常用content-type
1. text/html是html格式的正文
2. text/plain是无格式正文
3. text/xml忽略xml头所指定编码格式而默认采用us-ascii编码
4. application/xml会根据xml头指定的编码格式来编码
5. application/json
6. application/x-www-form-urlencoded：浏览器的原生form表单，如果不设置enctype属性，那么最终就会以这种方式提交数据。
7. multipart/form-data：我们使用表单上传文件时，让表单的enctyped等于multipart/form-data时，使用这种格式。示例：
```
POST http://www.example.com HTTP/1.1
Content-Type:multipart/form-data; boundary=----WebKitFormBoundaryrGKCBY7qhFd3TrwA

------WebKitFormBoundaryrGKCBY7qhFd3TrwA
Content-Disposition: form-data; name="text"

title
------WebKitFormBoundaryrGKCBY7qhFd3TrwA
Content-Disposition: form-data; name="file"; filename="chrome.png"
Content-Type: image/png

PNG ... content of chrome.png ...
------WebKitFormBoundaryrGKCBY7qhFd3TrwA--
```

## CDN(内容分发网络)
其基本思路是尽可能避开互联网上有可能影响数据传输速度和稳定性的瓶颈和环节，使内容传输地更快、更稳定。
长缓存：`Cache-Control: max-age=2592000`


## sass注释内容开始处加上“！”能免于被压缩，用于版权信息的声明

## jade
```jade
- var data = 'text'
p #{data}
p !{data}
p= data
p!= data
p \#{data}
p \!{data}
input(value=newData)
```


## 域名解析步骤
本地：
1. 搜索浏览器DNS缓存
2. 搜索操作系统DNS缓存
3. 读取本地HOST文件
4. 浏览器发起一个DNS的系统调用

DNS服务器：
1. 宽带运营商服务器查看本身缓存
2. 运营商服务器发起一个迭代DNS解析的请求
 1. 运营商服务器把结果返回操作系统内核，同时缓存起来
 2. 操作系统内核把结果返回浏览器
 3. 最终浏览器拿到了对应的IP地址