---
title: shadowsocks-libev
date: 2017-01-16 22:40:54
categories: Linux
tags:
  - shadowsocks
  - Linux
---

> Shadowsocks-libev is a lightweight secured SOCKS5 proxy for embedded devices and low-end boxes.

<!-- more -->

## shadowsocks 的隐蔽性
> local 和 server 端之间不需建立 SSH Tunnel，经过 GFW 的时候是常规的 TCP 包，没有明显的特征码

通常来说基于证书的身份认证过程和密钥交换过程都会带来独特的协议指纹，从而使得他们在 handshake 阶段就被 GFW 识别出来并阻断了；但 ShadowSocks 直接放弃了服务器端身份认证，也抛弃了密钥协商过程（ TLS 连接就是在 handshake 阶段协商出随机密钥的），而是采取事先在服务器端设置好固定密钥的方式来应对加密连接，这样做就大大减少了协议特征。


## 开启 TCP Fast Open
### 启用系统 TCP Fast Open

```bash
# 查看内核版本，确保不低于3.7
uname -a

# 临时生效
echo 3 > /proc/sys/net/ipv4/tcp_fastopen

# 一劳永逸
echo "net.ipv4.tcp_fastopen = 3" >> /etc/sysctl.conf
sysctl -p

# 检查是否开启成功
# net.ipv4.tcp_fastopen = 3
sysctl net.ipv4.tcp_fastopen
```

### shadowsocks 客户端和服务端开启 TFO
主要有以下两种思路：
1. 添加启动参数 `--fast-open`
2. 配置文件中添加 `"fast_open": true`

## 开启一次性认证
主要有以下两种思路：
1. 添加启动参数 `-A`
2. 配置文件中添加 `"auth": true`

### 注意点
**若服务端开启了“一次性认证”功能，则客户端也需开启此功能，否则 shadowsocks-libev 将无法正常工作。**


## Debian 8.6 简易部署脚本
<script src="https://gist.github.com/DevinZhong/6f953345c58c74cef8f970027b023d8e.js"></script>
关于此脚本的进一步说明，请访问[我的仓库](https://github.com/DevinZhong/scripts/tree/master/shadowsocks)