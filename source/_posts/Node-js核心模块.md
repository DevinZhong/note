---
title: Node.js核心模块
copyright: true
date: 2016-10-02 01:49:34
tags: Node.js
---

#### os
```js
var os = require("os");
var result = os.platform(); //查看操作系统平台
           //os.release(); 查看操作系统版本
           //os.type();  查看操作系统名称
           //os.arch();  查看操作系统CPU架构
console.log(result);
```

---
#### fs
```js
// fs.writeFile(filename, data, callback)
var fs= require("fs");
fs.writeFile('test.txt', 'Hello Node', function (err) {
   if (err) throw err;
   console.log('Saved successfully'); //文件被保存
});

// 往文件追加内容。如果文件不存在，则会创建一个新的文件
fs.appendFile(文件名,数据,编码,回调函数(err));
fs.appendFile('message.txt', 'data to append', function(err) {
  if (err) throw err
  console.log('Append to file successfully')
})

// 判断文件是否存在，exists为布尔型，表示文件是否存在
fs.exists('/etc/passwd', function(exists){
	console.log(exists ? '存在' : '不存在')
})

// 重命名文件
fs.rename(旧文件，新文件，回调函数(err){
   if (err) throw err;
   console.log('Successful modification,');
});

// 可以通过rename函数来达到移动文件的目的
fs.rename(oldPath,newPath,function (err) {
   if (err) throw err;
   console.log('renamed complete');
});

// 读取文件
fs.readFile(文件,编码,回调函数);

// unlink函数删除文件
fs.unlink(文件,回调函数(err));

// 创建目录
// 权限：可选参数，只在linux下有效，表示目录的权限，默认为0777，表示文件所有者、文件所有者所在的组的用户、所有用户，都有权限进行读、写、执行的操作。
fs.mkdir(路径，权限，回调函数(err));

// 删除目录
fs.rmdir(path, function(err){})

// 读取目录
fs.readdir(dir, callback(err, files){
	//files是一个存储目录中所包含的文件名称的数组，数组中不包括'.'和'..'
})
```

```js
//同步读取文件
var buffer = fs.readFileSync('/path/to/file')
//异步读取文件
fs.readFile('/path/to/file', function(err, data){/* data为buffer */})
fs.readFile('path/to/file', 'utf8', function(err, data){/* data为string */})
//读取目录
fs.readdir('/path/to/dir', function(err, list){/* list为文件名数组 */})
//管道处理流
fs.createReadStream('path/to/file').pipe(res);
```

---
#### net
```js
var server = net.createServer(function(socket){
	//socket处理逻辑
})
server.listen(8000)
```

---
#### url
```bash
node -pe "require('url').parse('/test?q=1', true)" 
```
```js
url.parse('https://imooc.com/course/list?name=devin&id=2', true, true)
url.format(urlObj)
url.resolve('https://imooc.com', '/course/list')
// 将字符串解析成url
// 第一个参数需要带协议，否则无效
// 第二个参数若以`/`打头，直接切到主机名后
// 第二个参数不以`/`打头，切到第一个参数最后一个`/`后（若主机名后没有`/`，切到主机名后）
url.resovle('http://imooc.com', '/course/list')
```

---
#### querystring
```js
// 将对象转换成查询字符串
// querystring.stringify(obj, '分隔符', '分配符')
querystring.stringify({foo: 'bar', cool: ['xyz', 'abc']})

// 反序列化
querystring.parse('查询字符串', '分隔符', '分配符')

querystring.stringify({name:'scott', course:['jade', 'node'], from''})
querystring.stringify({name:'scott', course:['jade', 'node'], from''}, ',', ':') //arg2是参数之间的分割符，arg3是参数名与值间的连接符
querystring.parse('name=scott&course=jade&course=node&from=')
querystring.parse('name:scott,course:jade,course:node,from:',',',':')
querystring.escape('<哈哈>')
querystring.unescape('%3C%E5%93%88%E5%93%88%3E')
```

---
#### path
```js
// 转换为标准路径
// 结果为 '/path/normalize/'
path.normalize('/path///normalize/hi/..'
)

// 拼接路径
// '/you/are/beautiful'
path.join('///you', '/are', '//beautiful')

// 返回路径中的目录名
var dirname = path.dirname('/foo/strong/cool/nice')
console.log(dirname) //'/foo/strong/cool'

// 返回最后一段，basename
path.basename('/foo/basename/index.html')

// 返回扩展名，以'.'打头
path.extname('/foo/extname/index.html')
```

---
#### util
```js
//将任意对象转换为字符串，通常用于调试和错误输出
util.inspect(object,[showHidden],[depth],[colors])

// format
// s:字符串 d:数字（整形和浮点型）%j:json
util.format('%s:%s', 'foo') //'foo:%s'
util.format('%s:%s', 'foo', 'bar', 'baz') //'foo:bar baz'
util.format(1, 2, 3) //'1 2 3'

// 判断是否为数组
util.isArray(object)

// 判断对象是否为日期类型
util.isDate(object)

// 判断对象是否为正则类型
util.isRegExp(object)
```

---
#### child_process
```js
// spawn
var child = child_process.spawn(command);
child.stdout.on('data', function(data){
	console.log(data)
})

// exec
child_process.exec(command, function(err, stdout, stderr){
	console.log(stdout)
})

// execFile 直接执行文件
child_process.execFile(file, function(err, stdout, stderr){
	console.log(stdout)
})

// fork 直接运行node模块
child_process.fork(modulePath)
```