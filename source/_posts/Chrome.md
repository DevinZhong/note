---
title: Chrome
copyright: true
date: 2016-09-27 21:54:42
tags: Chrome
---

## 导入密钥
```bash
wget https://dl.google.com/linux/linux_signing_key.pub
sudo rpm --import linux_signing_key.pub
```

## 使用代理
```bash
google-chrome --proxy-server="socks5://127.0.0.1:1080"
```

## 插件
OneTab extension for Google Chorme
session Buddy
FreshStart
Ctrl + Shift + D 保存所有当前页

## DNS缓存
chrome://net-internals/#dns

## plugins
chrome://plugins/

## service worker 相关
### 展示当前设备上激活和存储的 service worker
chrome://inspect/#service-workers
### 开始/暂停/调试 worker 的进程
chrome://serviceworker-internals

## components
全局代理下更新 flash，更新后重启即可

chrome://components/

## 允许过期插件
或许已失效
```bash
--allow-outdated-plugins
```