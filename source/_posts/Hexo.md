---
title: Hexo
copyright: true
date: 2016-09-27 01:55:01
tags: Hexo
---

## 常用简写
```bash
# hexo new
hexo n

# hexo generate
hexo g

# hexo server
hexo s

# hexo deploy
hexo d

# 生成部署
hexo d -g

# 生成预览
hexo g -s
```


## 常用命令
```bash
# 本地服务器
# 自带 watch 功能
hexo s

# 查看系统环境
hexo -v
```

## 常见错误解决方案

### 双大括号解析错误
此错误常见于行内代码
```md
`{% raw %}含有双大括号的内容{% endraw %}`
```