---
title: Systemd
date: 2017-01-15 22:33:35
categories: Linux
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
