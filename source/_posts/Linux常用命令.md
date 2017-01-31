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

# t 选项：强制分配伪终端，可以在远程机器上执行任何全屏幕（screen-based）程序
ssh -t peter@happycasts.net 'touch a.txt'
```

## rsync
```bash
# mydir 后面不能有 /
# happycasts: 不能没有 :
rsync -r mydir happycasts.net:
rsync -r happycasts.net:mydir .

# 同步目录下的文件
# 都必须有 /
rsync -r mydir/ happycasts.net:mydir/

rsync -av mydir/ happycasts.net:mydir/
# 将删除信息也同步上去
rsync -av --delete mydir/ happycasts.net:mydir/
# 打印信息
rsync -av --delete mydir/ happycasts.net:mydir/ --dry-run
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

## df 查看磁盘分区使用情况
| 选项 | 功能 |
|-----|------------------------------|
| l | 仅显示本地磁盘（默认） |
| a | 显示所有文件系统的磁盘使用情况，比如包含`/proc/` |
| h | 以 1024 进制计算最合适的单位显示磁盘容量 |
| H | 以 1000 进制计算最合适的单位显示磁盘容量 |
| T | 显示磁盘分区类型 |
| t | 显示指定类型文件系统的磁盘分区 |
| x | 不显示指定类型文件系统的磁盘分区 |


## du 统计磁盘上的文件大小
| 选项 | 功能 |
|-----|------------------------------|
| b | 以 byte 为单位统计文件 |
| k | 以 KB 为单位统计文件 |
| m | 以 MB 为单位统计文件 |
| h | 按照 1024 进制以最合适的单位统计文件 |
| H | 按照 1000 进制以最合适的单位统计文件 |
| s | 指定统计目标 |

## mount
| -o 选项参数 | 说明 |
|------------|-------------------------------|
| atime/noatime | 访问分区文件时，是否更新文件的访问时间，默认为更新 |
| async/sync | 默认为异步 |
| defaults | `mount -a`命令执行时，是否会自动安装`/etc/fstab`文件内容挂载，默认为自动 |
| exec/noexec | 定义默认值，相当于rw，suid，dev，exec，auto，nouser，async这七个选项 |
| remount | 重新挂载已经挂载的文件系统，一般用于指定修改特殊权限 |
| rw/ro | 文件系统挂载时，是否具有读写权限，默认是rw |
| suid/nosuid | 设定文件系统是否具有SUID和SGID的权限，默认是具有 |
| user/nouser | 设定文件系统是否允许普通用户挂载，默认是不允许，只有root可以挂载分区 |
| usrquota | 写入代表文件系统支持用户磁盘配额，默认不支持 |
| grpquota | 写入代表文件系统支持组磁盘配额，默认不支持 |
