---
title: npm
date: 2016-10-02 01:40:13
tags:
  - npm
  - Node.js
categories: JavaScript
---

## 使用淘宝源

### 临时使用
```bash
npm --registry https://registry.npm.taobao.org install express
```

### 持久使用
```bash
npm config set registry https://registry.npm.taobao.org

# 配置后可通过下面方式来验证是否成功
npm config get registry
```

## 模块版本规则
- 尖括号是一个比较宽松的对版本的限制，它只限制主版本号 ^0.7.3
- 波浪号是一个比较严格的对版本的限制，它限制到次版本号 ~0.4.1


## 常用命令
```bash
npm un hexo-renderer-marked -S
npm i hexo-renderer-markdown-it -S
```


## npm脚本

### npm短语
- test
- start
- stop

### npm脚本可以加上前置和后置钩子
- pretest
- posttest


## 查看安装包版本
```bash
# 查看generator版本
npm ls -g --depth=1 2>/dev/null | grep generator-
```

## 解决方案

### Vagrant 下建立软链接出错
- 一次性简单粗暴法
```bash
npm install --no-bin-links
```
- 一劳永逸简单粗暴法
```bash
npm config set bin-links false
```
- 正确解决姿势
 1. 编辑 Vagrantfile
   ```
   config.vm.provider "virtualbox" do |v|
     v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/vagrant", "1"]
   end
   ```
 2. 管理员模式启动 babun


### 写死并打包依赖
```json
{
  "scripts": {
    "pack": "npm shrinkwrap & shrinkpack ."
  }
}
```
