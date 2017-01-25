---
title: Linux常用命令
copyright: true
date: 2016-09-28 15:59:12
tags: Linux
---

> 待整理的 Linux 命令
<!-- more -->

## 端口占用情况
```bash
netstat -tlnp
```

## 系统信息
```bash
uname -a
```


## 语言相关
```bash
#查看安装的语系
locale -a
#安装中文
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
Ctrl + +： 放大字体 
Ctrl + -： 缩小字体 
j： 向下滚屏 
k： 向上滚屏 
g： 光标到文件头 
G： 光标到文件尾 
q： 退出 

 
## 查找
locate 系统全局： 
locate --regexp xxx(正则) 
sudo updatedb： 手动更新数据库 
find 目录翻个底朝天： 
find . | grep .txt 
find . -type f(或d) 
find . -type f -exec ls -l '{}' ';' 
find . -type f -exec grep -ni hello '{}' ';' -print 
n:行号 
i:忽略大小写 
-print:文件名 
 
## 常用
ls -ld mydir #查看目录信息 
ps aux  #查看所有进程状态 
命令之后加上 & ： 后台执行 
Ctrl + Z： 移至后台并暂停 
bg： 后台任务执行 
fg： 将后台任务移至前台 
kill -9： 强杀异常进程 #kill -9 5029 
kill -15： 默认方式 
kill -2： 相当于Ctrl + C 
ln -s happygrep hg 
file a.txt 
source peter.sh 在当前shell中执行
 
## ssh
```bash
ssh peter@happycasts.net

# 默认将密钥生成到 ~/.ssh 里
# 私钥： id_rsa
# 公钥： id_rsa.pub
ssh-keygen

# 将公钥上传到服务器
# 或许要在 ~/.ssh 目录下（未测试）,上传后在服务器上名为： ~/.ssh/authorized_keys
ssh-copy-id peter@happycasts.net
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

 
## 编译安装通常步骤
```bash
./configure 
make 
sudo make install 
```

## 制作启动U盘
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