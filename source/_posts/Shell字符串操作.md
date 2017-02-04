---
title: Shell字符串操作
date: 2017-02-04 00:03:43
tags: Linux
categories: Linux
---

> 在 Shell 编程中，字符串处理是很常见的问题。Shell 自带的字符串操作符速度很快，并且足够强大，我们需要牢牢掌握。
<!-- more -->


## 字符串取值的判断和替换
> `:`可以理解为更加苛刻，变量需要真的有值
- 对于 `-`、`=`，真的有值才能返回
- 对于 `+`，真的有值才被替换
- 对于 `?`，真的有值才不报错

表达式 | 含义
---------------|-----------------------------------------------------------
${var} | 返回变量var的值，与$var相同
${var-string} | 如果var未定义, 返回string
${var:-string} | 如果var未定义,或者其值为空，返回string
${var=string} | 如果var未定义, 返回string，并将var赋值为string
${var:=string} | 如果var未定义,或者其值为空，返回string，并将var赋值为string
${var+string} | 如果var已定义，返回string
${var:+string} | 如果var已定义，且其值不为空，返回string
${var?string} | 如果var未定义, 将string作为错误打印
${var:?string} | 如果var未定义,或者其值为空，将string作为错误打印


## 字符串长度
```bash
# 使用bash获取字符串长度
${#string}

# 使用wc命令
echo -n $string | wc -m

# 使用强大的expr命令
expr length $string
```

## 查找子串的位置
```bash
str="abc"
expr index $str "a"  # 1
expr index $str "b"  # 2
expr index $str "x"  # 0
expr index $str ""   # 0
```

## 字符串截取
```bash
# 在$string中从位置$position开始提取子字符串
# 如果$string是"*"或者"@"，那么将会从$position位置开始提取参数
${string:position}

# 在$string中从位置$position开始提取$length长度的子串
${string:position:length}

# 使用expr
str="abcdef"
expr substr "$str" 4 5  # 从第四个位置开始取5个字符，def
```

## 字符串剔除
- 一个井号（#）表示从最左边剔除最短的匹配
- 两个井号（##）表示从最左边剔除最长的匹配
- 一个百分号（%）表示从右边截取最短的匹配
- 两个百分号表示（%%）表示从右边截取最长的匹配

```bash
str="abbc,def,ghi,abcjkl"
echo ${str#a*c}     # ,def,ghi,abcjkl
echo ${str##a*c}    # jkl
echo ${str#"a*c"}   # abbc,def,ghi,abcjkl
echo ${str#*a*c}    # ,def,ghi,abcjkl
echo ${str#*a*c*}   # ,def,ghi,abcjkl
echo ${str#d*f}     # abbc,def,ghi,abcjkl
echo ${str#*d*f}    # ,ghi,abcjkl

echo ${str%a*l}     # abbc,def,ghi,
echo ${str%%b*l}    # a
echo ${str%a*c}     # abbc,def,ghi,abcjkl
```

## 字符串替换 
```bash
str="apple, tree, apple tree"
# 替换第一次出现的apple
echo ${str/apple/APPLE}
# 替换所有apple
echo ${str//apple/APPLE}

# 如果字符串str以apple开头，则用APPLE替换它
echo ${str/#apple/APPLE}
# 如果字符串str以apple结尾，则用APPLE替换它
echo ${str/%apple/APPLE}
```

## 比较
```bash
[[ "a.txt" == a* ]]        # 逻辑真 (pattern matching)
[[ "a.txt" =~ .*\.txt ]]   # 逻辑真 (regex matching)
[[ "abc" == "abc" ]]       # 逻辑真 (string comparision) 
[[ "11" < "2" ]]           # 逻辑真 (string comparision), 按ascii值比较
```

## 连接
```bash
s1="hello"
s2="world"
# $s1$s2 的写法不推荐
echo ${s1}${s2}
```