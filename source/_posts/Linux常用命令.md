---
title: Linux常用命令
copyright: true
date: 2016-09-28 15:59:12
categories: Linux
tags: Linux
---

> 待整理的 Linux 命令
<!-- more -->

## 查看端口占用情况
```bash
netstat -lnpt
```

## 系统信息
```bash
uname -a
```


## 语言相关
```bash
# 查看安装的语系
locale -a
# 安装中文
sudo locale-gen zh_CN.UTF-8
```


## 压缩与解压
```bash
# 将目录打包成 .zip
zip -r xxx.zip xxx/

# 解压 .tar.gz
tar zxvf xxx.tar.gz
# 打包 .tar.gz
tar zcvf xxx.tar.gz xxx/

# 解压 .tar.bz2
tar jxvf xxx.tar.bz2
# 打包 .tar.bz2
tar jcvf xxx.tar.bz2 xxx/
```
 
 
## less分页工具（man底层基于less）
- Ctrl + +： 放大字体 
- Ctrl + -： 缩小字体 
- j： 向下滚屏 
- k： 向上滚屏 
- g： 光标到文件头 
- G： 光标到文件尾 
- q： 退出 


## locate 系统全局查找
```bash
# 使用正规式查找
locate --regexp <regexp>

# 更新 locate 依赖的数据库
sudo updatedb
```

## find
```bash
# 与 grep 配合使用
find . | grep .txt
# 按类型查找
find . -type f
find . -type d

# exec 选项
find . -type f -exec ls -l '{}' ';'
## n 选项（grep）：显示行号
## i 选项（grep）：忽略大小写
## -print：同时输出文件名
find . -type f -exec grep -ni hello '{}' ';' -print
```

## ls
```bash
# 查看目录本身信息
ls -ld <dir>
```
 
## ssh
```bash
# 默认将密钥生成到 ~/.ssh 里
# 私钥： id_rsa
# 公钥： id_rsa.pub
ssh-keygen

# 将公钥上传到服务器，将公钥添加到服务器的 ~/.ssh/authorized_keys 文件中
ssh-copy-id devin@terraria.way2hacker.com
```


## 软件安装
```bash
sudo dpkg -i xxx.deb

# 所有安装过的deb包
dpkg -l

# 具体安装了什么
dpkg -L sublimetext

# 文件安装自哪个包
dpkg -S /opt/google/chrome

# 连配置文件一同删除
sudo apt-get purge git

apt-cache search ncurse | less 
```


## dd 制作启动U盘
```bash
sudo dd if=~/Download/openSUSE-Leap-42.1-DVD-x86_64.iso of=/dev/sdb bs=4M
```

## nohup 输出重定向
```bash
# 只输出错误信息到日志
nohup ./program >/dev/null 2>log &

# 所有输出都丢到黑洞
nohup ./program >/dev/null 2>&1 &
```

## tail
显示文件后几行，默认 10 行。一般用于查看日志。

```bash
# 监视日志输出
tail -f nohup.out
```

## 查看内存使用情况
```bash
# h 选项为 human-readable
free -h
```

## 查看公网IP
```bash
# 仅返回IP
curl ifconfig.co
# 返回IP、位置及服务商
curl ip.gs
```