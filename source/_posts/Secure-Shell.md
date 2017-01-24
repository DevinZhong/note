---
title: Secure Shell
date: 2017-01-17 00:30:44
categories: Linux
tags:
  - Linux
---

> Secure Shell（縮寫為SSH），由IETF的網路工作小組（Network Working Group）所制定；SSH為一項建立在應用層和傳輸層基礎上的安全協定，為電腦上的Shell（殼層）提供安全的傳輸和使用環境。

<!-- more -->

## 密码短语的重要性
- 在没有输入密码短语的情况下，您的私钥未经加密就存储在您的硬盘上，任何人拿到您的私钥都可以随意的访问对应的 SSH 服务器。
- 如果您不是 root 用户，则该机器上的 root 用户可以完全拥有您的密钥对，因为他的权限是最大的。
