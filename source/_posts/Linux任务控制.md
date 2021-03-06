---
title: Linux任务控制
date: 2017-01-25 13:07:49
categories: Linux
tags:
  - Linux
---

> 在操作系统的使用中，任务控制是基本同时又非常重要的操作。我们如果掌握不好这些常用命令，我们将在这个系统中寸步难行。
<!-- more -->

## 前后台切换
### 相关命令
- `jobs`：查看后台任务（当前终端）
- `bg`：让任务在后台运行
- `fg`：将一个任务拉到前台执行

### 键盘组合键
`Ctrl-Z`：挂起当前任务，挂起后为暂停状态


## kill
> 发送一个信号给进程 (通常用作杀进程)

### 常用命令
```bash
# 指定信号 kill 一个进程
kill -SIGTERM 2333
# 指定信号的数值表示 kill 一个进程
kill -9 2333
# kill 一个作业
kill %2

# 列出所有信号
kill -l
```

### 示例
以停止nginx为例
```bash
ps -ef | grep nginx
# 从容停止
kill -QUIT <main-pid>
# 快速停止（相当于 kill 不制定信号类型）
kill -TERM <main-pid>
# 强制停止
pkill -9 nginx
# pid 文件保存着主进程号。pid 文件保存位置可以在 nginx.conf 配置，如果没指定则放在 nginx 的 logs 目录下。
kill '/usr/nginx/logs/nginx.pid'
```

### 常见信号表
| 信号 | 数值 | 处理 | 程序是否可监听并作出决定 | 快捷键 |
|-----|-----|------------------|--------|----------|
| HUP | 1 | 终端断线，让进程挂起，睡眠 | Yes | |
| INT | 2 | 中断 | Yes | Ctrl + C |
| QUIT | 3 | 退出 | | Ctrl + \ |
| TERM | 15 | 终止，正常的退出进程，`kill` 默认信号 | Yes | |
| KILL | 9 | 强制终止 | 由内核发出，No | |
| CONT | 18 | 继续（与STOP相反， fg/bg命令） | | |
| STOP | 19 | 暂停 | | Ctrl + Z |

### HUP 信号
```bash
# 更改配置而不需停止并重新启动服务
# 在对配置文件作必要的更改后，发出该命令以动态更新服务配置
# 根据约定，当您发送一个挂起信号（信号 1 或 HUP）时，大多数服务器进程（所有常用的进程）都会进行复位操作并重新加载它们的配置文件。
kill -HUP pid
```

## ps
> 把进程系统的列出 (注: 只有ps -A能列出系统所有进程, ps只能列出当前进程和它的子进程)
```bash
# 查看所有进程状态
ps aux
```