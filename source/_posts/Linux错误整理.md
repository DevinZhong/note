---
title: Linux错误整理
copyright: true
date: 2016-10-01 01:52:04
tags: Linux
---

## apt update 提示“由于没有公钥，无法验证下列签名”
```bash
# 6AF0E1940624A220 为提示缺少的 key
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6AF0E1940624A220
```
