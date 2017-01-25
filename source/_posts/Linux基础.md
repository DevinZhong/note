---
title: Linux基础
copyright: true
date: 2016-10-01 01:20:48
tags: Linux
---

## 基本理论
- 执行权限 `x` 决定能否使用 `cd` 进入目录
- `shell` 里 `exit` 相当于 `Ctrl + D`

## 通配符
- `*` 和 `?` 都不匹配 `/`
- `{}` 匹配一个以 `,` 分隔的列表，列表项目可以为空。如 `{,*/}*.abc` 匹配 `*.abc` 或 `*/*.abc`
- `!` 取反

## 重定向
- `>>` 相当于追加
- `>` 相当于覆盖
```bash
# 示例
cat file1 file2 > file
cat files.txt | uniq | grep txt | sort
```

## 常识
- 在UNIX世界，rc经常被用作程序之启动脚本的文件名。他是“run commands”的缩写。
- 根据Unix传统，程序执行成功返回0，否则返回大于0的数值。

