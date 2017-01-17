---
title: Systemd
date: 2017-01-15 22:33:35
tags:
  - Linux
  - Systemd
---

> systemd 是 Linux 下的一款系统和服务管理器，兼容 SysV 和 LSB 的启动脚本。systemd 的特性有：支持并行化任务；同时采用 socket 式与 D-Bus 总线式激活服务；按需启动守护进程（daemon）；利用 Linux 的 cgroups 监视进程；支持快照和系统恢复；维护挂载点和自动挂载点；各服务间基于依赖关系进行精密控制。

<!-- more -->

## 注意点
一旦修改配置文件，就要让 SystemD 重新加载配置文件，然后重新启动，否则修改不会生效。

```bash
# 修改配置文件...

# 重新加载配置文件
sudo systemctl daemon-reload
# 重启服务
sudo systemctl restart httpd.service
```

## target 与 runlevel 的对应关系
| Traditional runlevel | New target name | Symbolically linked to... |
|----------------------|-----------------|---------------------------|
| Runlevel 0           | runlevel0.target | poweroff.target |
| Runlevel 1           | runlevel1.target | rescue.target |
| Runlevel 2           | runlevel2.target | multi-user.target |
| Runlevel 3           | runlevel3.target | multi-user.target |
| Runlevel 4           | runlevel4.target | multi-user.target |
| Runlevel 5           | runlevel5.target | graphical.target |
| Runlevel 6           | runlevel6.target | reboot.target |

## 常见信号表
| 信号 | 数值 | 处理 | 快捷键 |
|-----|-----|------------------|----------|
| HUP | 1 | 终端断线，让进程挂起，睡眠 | |
| INT | 2 | 中断 | Ctrl + C |
| QUIT | 3 | 退出 | Ctrl + \ |
| TERM | 15 | 终止，正常的退出进程 | |
| KILL | 9 | 强制终止 | |
| CONT | 18 | 继续（与STOP相反， fg/bg命令） | |
| STOP | 19 | 暂停 | Ctrl + Z |

## HUP 信号
```bash
# 更改配置而不需停止并重新启动服务
# 在对配置文件作必要的更改后，发出该命令以动态更新服务配置
# 根据约定，当您发送一个挂起信号（信号 1 或 HUP）时，大多数服务器进程（所有常用的进程）都会进行复位操作并重新加载它们的配置文件。
kill -HUP pid
```
