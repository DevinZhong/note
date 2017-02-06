---
title: HTML
copyright: true
date: 2016-09-28 14:57:29
tags: HTML
---

> HTML 相关的杂七杂八的东西记录在这里
<!-- more -->

## 防止页面文字被选中
```html
<body onselectstart="return false;"></body>
```

## 重定向
```html
<meta http-equiv="Refresh" content="5;url=https://note.way2hacker.com" />
```


## <img> usemap 属性
一个带有可点击区域的图像映射
```html
<img src="planets.gif" width="145" height="126" alt="Planets" usemap="#planetmap">

<!-- img 元素中的 "usemap" 属性引用 map 元素中的 "id" 或 "name" 属性（根据浏览器），所以我们同时向 map 元素添加了 "id" 和 "name" 属性 -->
<map name="planetmap" id="planetmap">
  <area shape="rect" coords="0,0,82,126" href="sun.htm" alt="Sun">
  <area shape="circle" coords="90,58,3" href="mercur.htm" alt="Mercury">
  <area shape="circle" coords="124,58,8" href="venus.htm" alt="Venus">
</map>
```

## 杂项
- 请始终将正斜杠添加到子文件夹。假如这样书写链接：`href="http://www.w3school.com.cn/html"`，就会向服务器产生两次 HTTP 请求。这是因为服务器会添加正斜杠到这个地址，然后创建一个新的请求，就像这样：`href="http://www.w3school.com.cn/html/`"

## HTML5新特性
### html5规范不规定属性值一定要用引号括起来
```html
<input value="foo" readonly />
<input value=foo readonly />
```


## 模板
```html
<!doctype html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>

<body>
    <header>
        <nav></nav>
    </header>
    <section></section>
    <section></section>
    <section></section>
    <footer></footer>
</body>

</html>
```

## 表单模板
```html
<form>
  <fieldset class="account-info">
    <legend>Log in</legend>
    <label>
      Username
      <input type="text" name="username">
    </label>
    <label>
      Password
      <input type="password" name="password">
    </label>
  </fieldset>
  <fieldset class="account-action">
    <input class="btn" type="submit" name="submit" value="Login">
    <label>
      <input type="checkbox" name="remember"> Stay signed in
    </label>
  </fieldset>
</form>
```