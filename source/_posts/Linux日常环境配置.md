---
title: Linux日常环境配置
copyright: true
date: 2016-09-27 22:02:27
tags: Linux
---

## Ubuntu配置开机启动
```bash
gnome-session-properties
```

## Ubuntu服务端安装shadowsocks
1. 编译安装shadowsocks-libev
2. 编辑配置文件
3. `service shadowsocks-libev start`
4. 如果没法使用，尝试restart，或者加密方式改为`rc4-md5`


## shadowsocks-libev 后台运行
```bash
nohup ss-local -c /etc/shadowsocks-libev/config.json >/dev/null 2>&1 &
```


## bash下使用proxychains代理
1. 根据github说明安装
2. 将`/etc/proxychains.conf` 末尾 `socks4 127.0.0.1 9095` 改为 ss的 `socks5 127.0.0.1 1080`
3. `proxychains4 curl ip.gs` 命令可验证代理是否可用


## unblock华硕的杂牌无线网卡
```bash
# 临时生效
# modprobe 可载入指定的个别模块，或是载入一组相依的模块。
# modprobe 常用语启用模块
# 这里 -r 指 remove， 移除 acer-wmi 模块
sudo modprobe -r acer-wmi

# 一劳永逸
echo "blacklist acer_wmi" | sudo tee -a /etc/modprobe.d/blacklist.conf
reboot
```

## hostname修改
```bash
# 这两个文件都与 hostname 相关联
vim /etc/hostname
vim /etc/hosts
```

## openSUSE快捷键配置

### 区域截屏
自定义快捷键添加如下命令：
```bash
spectacle -r
```

### 显示桌面
1. 打开系统设置窗口
2. 选择快捷方式和手势
3. 点击左边栏中的全局键盘快捷键
4. KDE组件选择 “kwin”
5. 在搜索里输入“显示桌面”
6. 点击搜索结果列表中的“显示桌面”
7. 选择自定义，设置快捷键为 `Win + D`

## 运维常用配置
### 手工配置防火墙
```bash
vi /etc/sysconfig/iptables
/etc/init.d/iptables restart
```

### 关闭 SELINUX
```bash
rm -rf /etc/selinux/config
echo 'SELINUX=disabled' > /etc/selinux/config
```

### 配置常用服务开机启动
```bash
chkconfig nginx on
chkconfig mysqld on
chkconfig php-fpm on
```

### 限制 ssh
```bash
cat << EOF >> /etc/ssh/sshd_config
Port 10022
protocol 2
PermitRootLogin no
EOF
service sshd reload
ssh -p 10022 devin@host.com
```
