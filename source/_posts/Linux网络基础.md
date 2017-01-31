---
title: Linux网络基础
date: 2017-01-31 17:41:02
tags:
  - Linux
  - network
categories: Linux
---

> 网络特性对于一个服务器而言是至关重要的，Linux网络基础是我们的必修课。
<!-- more -->

## 网卡配置文件各项属性说明
| 属性名 | 说明 | 可选项 |
|---------|----------------------------|---------|
|DEVICE|网卡设备名| |
|BOOTPROTO|是否自动获取IP| none,static,dhcp |
|HWADDR|MAC地址| |
|ONBOOT|是否随网络服务启动| yes,no |
|TYPE|类型| Ethernet,xDSL,Bridge,IPSEC |
|UUID|唯一识别码| |
|IPADDR|IP地址| |
|NETMASK|子网掩码| |
|GATEWAY|网关| |
|DNS1|DNS| |
|IPV6INIT|IPv6是否启用| yes,no |
|IPV6_AUTOCONF|是否自动化配置IPv6| yes,no |
|USERCTL|是否允许非 root 用户控制此网卡| yes,no |


## 常用服务端口
| 服务 | 端口号 |
|---------------|--------|
| FTP | 20,21 |
| SSH | 22 |
| Telnet | 23 |
| DNS | 53 |
| HTTP | 80 |
| HTTPS | 443 |
| SMTP（简单邮件传输协议） | 25 |
| POP3（邮局协议3代） | 110 |