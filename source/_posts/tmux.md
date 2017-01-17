---
title: tmux
copyright: true
date: 2016-09-28 16:02:18
tags:
  - tmux
  - Linux
---

```bash
# 返回原来session
# 快捷键 Ctrl + B + D
tmux detach
# 返回后台运行的session
# 简写 tmux a
tmux attach
# 新建session
tmux new -s session_name
# 切换到指定session
tmux attach -t session_name
# 列出所有session
tmux list-sessions
# 杀死指定session
tmux kill-session -t session_name
```