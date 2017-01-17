---
title: Nginx
copyright: true
date: 2016-10-21 16:02:31
tags: Nginx
---

## 目前缺陷
- rewrite 功能不够强大
- 模块没有 Apache 多


## install
```bash
tar -zxvf nginx-1.8.0.tar.gz

yum -y install pcre pcre-devel zlib zlib-devel

./configure
make
make install

# 安装到 /usr/local/nginx

# 启动
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

## 关闭
```bash
ps -ef | grep nginx

# 从容停止
kill -QUIT 2072

# 快速停止
kill -TERM 2032
kill -INT 2032

# 强制停止
pkill -9 nginx
```

## 验证配置文件是否正确
```bash
/usr/local/nginx/sbin/nginx -t
/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
```

## 重启
```bash
/usr/local/nginx/sbin/nginx -s reload

# 或发送信号重启
kill -HUP 2255
```

## 信号控制
- HUP：重启
- QUIT：从容关闭
- TERM：快速关闭
- USR1：切换日志文件
- USR2：平滑升级可执行进程
- WINCH：从容关闭工作进程

## 从容关闭工作进程
```bash
kill -WINCH master-pid
```

## shengji
```bash
nginx -v

./configure
make
# 不用 make install
cp -rfp objs/nginx /usr/local/nginx/sbin

cd /usr/local/nginx/sbin
cp nginx nginx.old

```

## 配置
```txt
#user nobody
# cpu 核数或核数两倍
worker_processes 4;

pid logs/nginx.pid;

event {
  # 最大连接数
  worker_connection 1024;
}

http {
  gzip on;

  server {
    charset gb2312;
  }
  server {
    
  }
}
```

