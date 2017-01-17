---
title: Git
copyright: true
date: 2016-09-26 23:09:48
tags: Git
---

## 常用命令

### 提交
```bash
# 自动add已跟踪文件
git commit -am "auto add"
# 可以提交多行message
git commit -a
# 可以检查提交
git commit -a -v
```

### 分支
```bash
# 查看本地分支
git branch
# 查看远程分支
git branch -r
# 查看所有分支
git branch -a

# 在当前分支的基础上新建一个分支
# 相当于 git branch newBranch && git checkout newBranch
git checkout -b newBrach

# 在 origin/master 的基础上创建一个新分支
git checkout -b newBrach origin/master
# 切换到 master 分支
git checkout master
# 删除分支
git checkout -D tmp
```

### fetch
```bash
# 取回远程主机所有分支的更新
# git fetch <远程主机名>
git fetch origin

# 取回远程主机指定分支的更新
# git fetch <远程主机名> <远程分支名>
git fetch origin master
```

### rm
用 `git rm` 来删除文件，同时还会将这个删除操作记录下来；
用 `rm` 来删除文件，仅仅是删除了物理文件，没有将其从 git 的记录中剔除。

`git add .` 仅能记录添加、改动的动作，删除的动作需靠 `git rm` 来完成。
`rm` 删除的文件是处于 `not staged` 状态的，也就是一种介于 “未改动” 和 “已提交过” 之间的状态。

## 现成解决方案

### 上传已有项目
```bash
git remote add origin <repo>
git push -u origin master
```

### 建立pages分支
```bash
# orphan 分支与现存分支没有共同的历史
# 在第一次提交前，orphan分支不会出现在git branch分支列表中
git checkout --orphan gh-pages
# 请掉原工作目录的内容
git rm -rf .

# 编辑分支内容

# 提交
git add .
git commit -m 'create pages branch'
git push origin gh-pages
```

### pages分支绑定域名
添加两条A记录，分别指向 `192.30.252.153` 和 `192.30.252.154`

### Hexo主题版本管理（子仓库）

#### 建立关联
```bash
# -f 意思是在添加远程仓库之后，立即执行 fetch
git remote add -f <子仓库名> <子仓库地址>
# --squash 意思是把 subtree 的改动合并成一次 commit，这样就不用拉取子项目完整的历史记录
# --prefix之后的=等号也可以用空格
git subtree add --prefix=<本地子目录> <子仓库名> <子仓库分支> --squash
```

#### 更新子目录
```bash
git fetch <远程仓库名> <远程分支>
git subtree pull --prefix=<本地子目录> <远程仓库名> <远程分支> --squash
```

#### push子树
```bash
git subtree push --prefix=<本地子目录> <远程仓库名> <远程分支>
```

#### 切换子树分支
```bash
git rm <subtree>
git commit
git subtree add --prefix=<subtree> <repository_url> <branch>
```

### 删除Git的提交记录
```bash
# 删除最后一次的提交记录
git reset --hard HEAD^
# 删除最后3次的提交记录
git reset --hard HEAD~3

# 如果已经提交到远程仓库，还需要强行 push，注意别把别人的提交覆盖了
git push --force
```
