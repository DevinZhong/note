---
title: Node.js第三方模块
copyright: true
date: 2016-10-02 01:51:14
tags: Node.js
---

## bl和concat-stream
```js
response.pipe(bl(function(err, data){/* ... */}))
response.pipe(concatStream(function(data){/* ... */}))
```

## strftime
```js
console.log(strftime('%B %d, %Y %H:%M:%S')) // => April 28, 2011 18:21:08
console.log(strftime('%F %T', new Date(1307472705067))) // => 2011-06-07 18:51:45
```

## through2-map
```js
var map = require('through2-map')
// 字符串反转
inStream.pipe(map(function(chunk){
	return chunk.toString().split('').reverse().join('')
})).pipe(outStream)
// 大写转换器
req.pipe(map(function(chunk){
	return chunk.toString().toUpperCase();
})).pipe(res)
```

## socket.io
```js
// 服务端
var chat = io
    .of('/chat')
    .on('connection', function (socket) {
        socket.emit('a message', {  
        }); 
        chat.emit('a message', {  
        }); //相当于io.emit
      });

// 根据socket.id获取socket
io.sockets.connected[socketid].emit();

// 客户端
var socket  = io.connect("ws://localhost/chat");
```


## ab测试
```bash
ab -n1000 -c10 http://www.imooc.com/
```