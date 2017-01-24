---
title: vim
copyright: true
date: 2016-10-01 01:35:55
tags:
  - vim
  - Linux
---

> Vim是從vi發展出來的一個文字編輯器。代碼補完、編譯及錯誤跳轉等方便編程的功能特別豐富，在程式設計師中被廣泛使用。和Emacs並列成為類Unix系統用戶最喜歡的編輯器。

## 模式切换
- `i`：插入
- `a`：添加
- `v`：visual mode

## 编辑操作
- `x`：删除一个字符
- `u`：撤销一个操作
- `dd`：删除一行
- `yy`：复制一行
- `dw`：删除一个单词（光标处到单词间隔）
- `yw`：复制一个单词（光标处到单词间隔）
- `p`：粘贴
- `e`：跳到下一个“单词尾部”
- `r`：替换一个字符
- `ZZ`：保存退出
- `G`：跳转到末行
- `gg`：跳转到首行

## 高级命令
- `:sp 文件名`：水平分屏
- `:vsp 文件名`：垂直分屏
- `:! 命令名`：不推出`vim`的同时，运行终端命令

## 杂项
- 配置文件位置：`~/.vimrc`
- 交互式教程：`vimtutor`
- 在线交互式教程收藏：[http://www.openvim.com/tutorial.html](http://www.openvim.com/tutorial.html)
