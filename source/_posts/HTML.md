---
title: HTML
copyright: true
date: 2016-09-28 14:57:29
tags: HTML
---


## 防止页面文字被选中
```html
<body onselectstart="return false;"></body>
```


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