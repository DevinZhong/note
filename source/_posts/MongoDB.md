---
title: MongoDB
copyright: true
date: 2016-10-02 01:07:44
tags: MongoDB
---

## 待整理
- mongod --config xxx.conf
- `db.shutdownServer()` 关闭服务
- 不要使用 `kill -9` 强杀MongoDB服务
- `tail` 命令适合查看日志
- `update()` 默认处理一条，用第四个参数处理多条
- `remove()`默认处理所有匹配数据


## 索引

### 比较重要的索引属性
- 名字
- 唯一性
- 稀疏性
- 是否定时删除

### 过期索引的限制
1. 存储在过期索引字段的值必须是指定的时间类型（ISODate或者ISODate数组，不能是时间戳），不符合则不能自动删除
2. 如果指定了ISODate数组，则按照最小的时间进行删除
3. 过期索引不能是复合索引
4. 删除时间是不精确的（删除过程是由后台程序每60s跑一次，而且删除也需要一些时间，所以存在误差）


### 全文索引的使用限制
1. 每次查询，只能指定一个$text查询
2. $text查询不能出现在$nor查询中
3. 查询中如果包含了$text,hint不再起作用
4. MongoDB全文索引还不支持中文


### 2dsphere索引
概念：球面地理位置索引
创建方式：db.collection.ensureIndex({w:"2dsphere"})
位置标示方式：
GeoJSON：描述一个点，一条线，多边形等形状
格式：
{type:"", coordinates:[<coordinates>]}
查询方式与2d索引查询方式类似，支持$minDistance与$maxDistance


### 如何评判当前索引构建情况
1. mongostat工具（qr|qw字段）
2. profile集合（`db.getProfilingLevel()`和`db.getProfilingStatus()`）
3. 日志
4. explain分析


## 权限

### 内建角色类型
- read：可查询
- readWrite：对文档的增删改
- dbAdmin：管理
- dbOwner：以上三个的结合体
- userAdmin：管理用户

### MongoDB 用户角色详解
1. 数据库角色（read,readWrite,dbAdmin,dbOwner,userAdmin）
2. 集群角色（clusterAdmin,clusterManager...）
3. 备份角色（backup,restore...）
4. 其他特殊权限（DBAdminAnyDatabase...）



### 用户管理：
```js
//创建用户
db.createUser({user:"imooc",pwd:"imooc",roles:[{role:"userAdmin",db:"admin"},{role:"read",db:"test"}]})
```


## 待整理

```
db.getProfilingLevel()
db.getProfilingLevel()
db.setProfilingLevel(2)
db.system.profile.find().sort({$natural:-1}).limit(1)
db.imooc_21.find({x:1}).explain()
```

```
show dbs
use imooc
db.dropDatabase() //use 之后
use imooc //无需先创建
db.imooc_collection.insert({x:1}) //建自动创建imooc库
show tables
show collections //查看表
db.imooc.count()
db.test.save({name:'relpSet_initiate'}) //有则覆盖，没有则插入

db.imooc_collection.find() // 默认查询所有
db.imooc_collection.find({x:1})
for(i=3;i<100;i++)db.imooc_collection.insert({x:i}) //支持js语法
db.imooc_collection.find().count()
db.imooc_collection.find().skip(3).limit(2).sort({x:1}) //**先sort()，再skip()和limit()，1正序，-1降序**

db.imooc_2.findOne()
db.imooc_2.insert({time:new Date()})

//更新操作
db.imooc_collection.update({x:1}, {x:999})
db.imooc_collection.update({z:100}, {$set:{y:99}}) //部分更新
db.imooc_collection.update({y:100}, {y:999}, true) //upsert，若没有记录，则生成
db.imooc_collection.update({c:1},{$set:{c:2}},false, true) //只能用set操作，为防止误操作。

//删除操作
db.imooc_collection.remove({c:2})
db.imooc_collection.drop() //删除表


//索引操作
db.imooc_collection.getIndexes()
db.imooc_collection.ensureIndex({x:1}) //创建
db.imooc_2.ensureIndex({x:1, y:1})
db.imooc_2.ensureIndex({time:1},{expireAfterSeconds:60}) //过期索引
db.imooc_21.ensureIndex({x:1,y:-1,z:1,m:1},{name:"normal_index"}) //索引命名
db.imooc_21.dropIndex("normal_index") //删除索引
db.imooc_21.ensureIndex({m:1, n:1},{unique:true}) //唯一索引（实现mysql：insert ignore）
db.imooc_21.find({m:{$exists:true}})
db.imooc_21.ensureIndex({m:1},{sparse:true}) //稀疏索引
db.imooc_21.find({m:{$exists:false}}).hint("m_1") //强制使用索引查找不存在的文档


//全文索引
//创建
db.imooc_2.ensureIndex({"article":"text"})
db.articles.ensureIndex({key_1:"text", key_2:"text"})
db.articles.ensureIndex({"$**":"text"}) //对集合中的所有字段创建一个大的全文索引

db.imooc_2.find({$text:{$search:"aa"}})
db.imooc_2.find({$text:{$search:"aa bb cc"}}) //“或”查询（空格分割）
db.imooc_2.find({$text:{$search:"aa bb -cc"}}) //包含aa或bb但是不包含cc
db.imooc_2.find({$text:{$search:"\"aa\" \"bb\" \"cc\""}}) //“与”查询（引号包围）

db.imooc_2.find({$text:{$search:"aa bb"}},{score:{$meta:"textScore"}}) //分数越高，相似度越大（这里的score是自己定义的key，不用固定）
db.imooc_2.find({$text:{$search:"aa bb"}},{score:{$meta:"textScore"}}).sort({score:{$meta:"textScore"}}) //排序

//地理位置索引
db.location.ensureIndex({"w":"2d"})
db.location.find({w:{$near:[1,1]}})
db.location.find({w:{$near:[1,1],$maxDistance:10}}) //最大距离
db.location.find({w:{$geoWithin:{$box:[[0,0],[3,3]]}}}) //矩形范围
db.location.find({w:{$geoWithin:{$center:[[0,0],5]}}}) //圆形范围
db.location.find({w:{$geoWithin:{$polygon:[[0,0],[0,1],[2,5],[6,1]]}}}) //多边形范围
//geoNear
db.runCommand({geoNear:"location",near:[1,2],maxDistance:10,num:2})
//2dsphere
```


## API
```js
//删除符合条件的所有文档
db.collection('restaurants').deleteMany({borough: 'Manhattan'}, function(err, results) {
	console.log(results);
    callback();
})

// 删除单一文档
db.collection('restaurants').deleteOne({borough: 'Queens'}, function(err, results) {
	console.log(results);
    callback();
})

// 删除所有文档
db.collection('restaurants').deleteMany({}, function(err, results) {
	console.log(results);
    callback();
})

// 删除整个集合
db.collection('restaurants').drop(function(err, response) {
	console.log(response);
    callback();
})
```

## MongoDB复制集

- oplog记录写操作，不记录读操作
- oplog： operation-log
- 特性：封顶表 Capped collection 滚动覆盖写入（不能从封顶表里删除数据）
- 默认大小：64位Linux，windows操作系统下为当前分区可用空间5%，体积不会超过50G
- 关键词：复制源，封顶表，大小可以定制


--oplogSize=4096(mb) 最小4k

### Oplog的结构
ts：操作发生时的时间戳（8字节时间戳）
h：此操作的独一无二的ID
v：oplog的版本
op：操作类型 **i**nsert, **u**pdate, **d**elete, db **c**md, **n**ull
ns：操作所处的命名空间 db_name.coll_name
o：操作对应的文档，文档在更新前的状态
o2：仅update操作时有，更新操作的变更条件


### 更改oplogSize
1. 将成员以单机模式启动
2. 将oplog最新的一条操作保存到临时表里
3. db.temp.save(db.oplog.rs.find().sort({$natural:-1}).limit(1).next())
4. 删除原来的oplog.rs集合 db.oplog.rs.drop()
5. 以创建封顶表的方式，创建新的oplog.rs db.createCollection('oplog.rs',{capped:true, size:1024*1024*1024*3})
6. 将之前保存的原oplog中最新的操作插入到新的oplog中
7. 将单机模式的节点返回到复制集当

db.oplog.rs.save(db.temp.findOne())


### 复制集初始化同步过程：
1. 检查本机的oplog
2. 标记同步源oplog中最新的操作为start点
3. Drop本机，复制同步源（clone）所有数据到本地（耗时）
4. 建立索引（耗时）
5. 标记同步源oplog中最新的操作未minValid
6. 执行同步源oplog中start点到minValid点的所有操作
7. 成为正常的复制集成员


```mongo
db.createCollection(
'coll_name',
{capped:true, size: 1024*1024*1024*4, max:5000} //max可选
)
```

### 复制集特点
- 数据一致性
 - 主是唯一的
 - 大多数原则（二分之一原则）：集群存活节点小于等于二分之一时集群不可写，只可读
 - 从库无法写入：MySQL从库的readonly对具有super权限的账户无效
 - 自动容灾

能否选举出新的主节点，是由当前复制集成员存活数量来决定的。
复制集的服务器挂了一半就没法选举了，将**全部**降为从节点。
复制集不支持只复制指定的库


### 五种**读**策略
只读主、优先主、只读从、优先从、就近（网络延迟）

---
- 数据节点：存放数据（实体物理文件*.ns, *.0等）的节点，包括主节点、从节点
- 投票节点：不存放数据仅作选举和充当复制集节点
```mongo
db.runCommand("shutdownServer")
```

隐藏节点不会被前端所发现
延时节点需要隐藏


### 按功能区分节点
主节点：提供读写服务的节点
从节点：提供读服务的节点
    - 隐藏节点：对程序不可见的节点
    - 延时节点：延时复制节点
    - **投票节点**：具有投票权的节点，不是arbiter
投票节点：Arbiter节点，无数据，仅作选举和充当复制集节点，也称它为选举节点

```bash
ps -ef | grep mongo
netstat -tlnp
```

### 复制集配置
```mongo
use admin #可以用db查看当前db
config = {
    _id:"IMOOC",
    members:[
        {_id:0, host:"192.168.1.105:28001"},
        {_id:1, host:"192.168.1.105:28002"},
        {_id:2, host:"192.168.1.105:28003"}]
    }
config.members[2] = {_id:2, host:"192.168.1.105:28003", arbiterOnly:true}
rs.initiate(config) //初始化
rs.status()
rs.conf()
//使从节点可读（从节点中）
rs.slaveOk(true) //或1

rs.reconfig(config)
```

```
rs.stepDown(5)

use local
db.oplog.rs.find().sort({$natural:-1}).limit(1).pretty()

```

.pretty()
rs.isMaster() //看不到隐藏节点

//推荐的添加节点方式：
rs.add()
rs.addArb()

config = {
... _id:'IMOOC',
... members:[
... {_id:0, host:'192.168.1.105:28001'}]
... }
rs.initiate(config)


db.test.save({name:'relpSet_initiate'}) //有则覆盖，没有则插入

即使（暂时）只有一台服务器，也要以单节点模式启动复制集
1. 单机多实例启动复制集
2. 单节点启动复制集

rs.freeze(100)


kill -2

show logs
show log rs

人工干预选举：
priority， rs.freeze()，rs.stepDown()

db.coll.ensureIndex({background:true})

use local
db.oplog.rs.stats(1048576)

```bash
du -shc local.* //新版不可行
```

选举四个阶段：心跳检测、维护主节点备选列表、选举准备、投票

### 主节点检测
```flow
cond1=>condition: 自己是否为主节点？
cond2=>condition: 能否看到大多数？
cond3=>condition: 是否有主节点？
cond4=>condition: 列表中是否存在比
当前主节点 priority
更高的节点？
cond5=>condition: opTime 与最新节点
差距是否不超过 10s？
op1=>operation: 与主节点备用列表比对
op2=>operation: opTime 比对
e1=>end: 主节点降级
e2=>end: 进入选举
准备环节

cond1(yes,right)->cond2
cond2(no)->e1
cond1(no)->cond3
cond3(yes)->op1->cond4
cond4(yes)->op2->cond5
cond5(yes)->e1
cond3(no)->e2
```

### 主节点备用列表维护
```flow
op=>operation: 心跳检测
cond1=>condition: 是否异常？
cond2=>condition: 自检：是否满足
大多数原则？
cond3=>condition: 自检：priority
是否大于等于 1？
cond4=>condition: 自检：status 是
否为 secondary
（否则为 arbiter）？
cond5=>condition: 自检：opTime与主的
差距在 10 秒内？
e1=>end: 将自己加入
主节点备选列表
e2=>end: 将自己移除
主节点备选列表
e3=>end: 全部变为从节点，
主节点也降级为从节点

op->cond1
cond1(yes)->cond2
cond2(yes)->cond3
cond3(yes)->cond4
cond4(yes)->cond5
cond5(yes)->e1
cond2(no)->e3
cond3(no)->e2
cond4(no)->e2
cond5(no)->e2
```

### 自选方法验证
```flow
cond1=>condition: 是否拿到线程锁？
cond2=>condition: 是否够资格？
cond3=>condition: opTime 是否合格？
e1=>end: 索取投票
e2=>end: 不索取投票

cond1(no)->e2
cond1(yes)->cond2
cond2(no)->e2
cond2(yes)->cond3
cond3(no)->e2
cond3(yes)->e1
```

### 选举准备（集群中无主节点）
```flow
cond1=>condition: 是否在列表中？
cond2=>condition: 是否看到大多数？
e1=>end: 调用“自选”方法
e2=>end: 退出流程

cond1(no)->e2
cond1(yes)->cond2
cond2(no)->e2
cond2(yes)->e1
```

### 投票
```flow
e1=>end: 上位成功
e2=>end: 上位失败
e3=>end: 重新投票
cond1=>condition: 是否有反对票？
cond2=>condition: 是否有票数一样的？
cond3=>condition: 是否票数过半？

cond1(yes,right)->e2
cond1(no)->cond2
cond2(yes,right)->e3
cond2(no)->cond3
cond3(no)->e2
cond3(yes)->e1
```


## 单实例复制集启动
1. 最好先清一下数据
2. ```config={_id:"IM",members:[{_id:0,host:"127.0.0.1:27017"}]}```
3. rs.initiate(config)