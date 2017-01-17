---
title: Node.js基础
copyright: true
date: 2016-10-02 01:47:00
tags: Node.js
---

如果是 post 传来的 body 数据，则是在 req.body 里面，不过 express 默认不处理 body 中的信息，需要引入 https://github.com/expressjs/body

---
- 我们习惯中的Node.js回调函数都有像如下所示的特征：
```js
function callback(err, data){/*...*/}
```
- 模块必须遵守的原则：
  - 当有错误发生，或者有数据的时候，准确调用回调函数
  - 不要改变其他的任何东西，比如全局变量或者stdout
  - 处理所有可能发生的错误，并把它们传递给回调函数
  - 尽早捕获错误，并在回调中返回：
  	```js
  	function bar (callback) {  
       foo(function (err, data) {  
         if (err)  
           return callback(err) // 尽早返回错误  
       
         // ... 没有错误，处理 `data`  
       
         // 一切顺利，传递 null 作为 callback 的第一个参数  
       
         callback(null, data)  
       })  
     } 
  	```
- 'data'事件会在每个数据块到达并已经可以对其进行一些处理的时候被触发。数据块的大小将取决于数据源。

```js
//取出传入的q参数
var q = req.query.q;
//使用utility模块
var utility = require('utility');
var md5Value = utility.md5(q)
var sha1Value = utility.sha1(q)
```

---
#### 踩过的坑
- fs.readFileSync(fileName)读出来的buffer会多出一个'\n'

---
#### 通配符
- *和?都不匹配/
- **能匹配/ 任意数量任意字符
- {a,b}.js a或b 匹配一个list
- ! 取反



---
模块对外暴露`module.exports`，在模块中可以简写成`exports`（相当于内部有一个`var exports = module.exports`引用），但是不能将`exports`覆盖输出，因为这样`exports`就引用不到`module.exports`了

一个Buffer大概64KB
四大流：Readable、Writable、Duplex、Transform

---
require加载顺序
1. 核心模块
2. 相对路径文件模块
3. 绝对路径文件模块
4. 非路径模块
- node_modules层层向外

---
require默认扩展格式
1. .js
2. .json
3. .node
4. 其他

---
### CommonJS
```js
(function(exports, require, module, __filename, __dirname){
	var math = require('math');
    exports.area = function(radius){
    	return Math.PI * radius * radius;
    }
})
```

---
#### Grunt
```bash
# 指定target
grunt sass:dist
# 附加参数
grunt serve --allow-remote
```

## 内置对象
---
#### process
```js
// 获取应用程序当前目录
process.cwd()

// 改变应用程序目录
process.chdir('目录')

// 将内容打印到输出设备上
console.log = function(d){
	process.stdout.write(d + '\n')
}

// 标准错误输出
process.stderr.write('内容')

// 通过注册事件的方式来获取输入的内容
process.stdin.on('readable', function(){
	var chunk = process.stdin.read();
    if(chunk != null){
    	process.stdout.write('data: ' + chunk);
    }
})

// 程序内杀死进程，退出程序。`code`为退出后返回的代码，如果省略则默认返回0
process.exit(code)

// 监听进程exit事件，在程序退出前进行清理工作
// 参数code表示退出码
process.on('exit', function(){
	//进行一些清理工作
})

// 监听uncaughtException事件，让进程优雅地退出
// 参数err表示发生的异常
process.on('uncaughtException', function(err){
	console.log(err)
})
throw new Error('我故意的...')

// 为流设置编码
process.stdin.setEncoding(编码);
process.stdout.setEncoding(编码);
process.stderr.setEncoding(编码);
```