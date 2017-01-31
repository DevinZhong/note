---
title: AJAX
copyright: true
date: 2016-10-02 01:27:15
tags:
  - AJAX
  - JavaScript
---

> AJAX 技术的应用，使 WEB 的用户体验产生质的飞跃
<!-- more -->

## 使用XMLHttpRequeste的经典AJAX
### XMLHttpRequest 实例属性
| 属性 | 说明 |
|---------------|----------------------------|
| responseText | 获得字符形式的响应数据 |
| responseXML | 获得 XML 形式的响应数据 |
| status | 以数字形式返回的 HTTP 状态码 |
| statusText | 以文本形式返回的 HTTP 状态码 |
| getAllResponseHeader() | 获取所有的响应报头 |
| getResponseHeader() | 查询响应中的某个字段的值 |
| readyState | 当前请求处理阶段的数字表示，有 5 种可能取值 |

### XMLHttpRequest 实例的 readyState 属性详解
| 数值 | 说明 |
|------------------|-------------------------|
| 0 | 请求未初始化，open 还没有调用 |
| 1 | 服务器连接已建立，open 已经调用了 |
| 2 | 请求已接受，也就是接收到头信息了 |
| 3 | 请求处理中，也就是接收到响应主体了 |
| 4（XMLHttpRequest.DONE） | 请求已完成，且响应已就绪，也就是响应完成了 |

### 示例代码
```js
/*
兼容老版本IE
var request;
if (window.XMLHttpRequest) {
    request = new XMLHttpRequest(); // IE7+
} else {
    request = new ActiveXObject("Microsoft.XMLHTTP"); // IE6,IE5
}
*/
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    // xhr.readyState == XMLHttpRequest.DONE && xhr.status === 200
    if (xhr.readyState == XMLHttpRequest.DONE) {
        alert(xhr.responseText);
    }
}
xhr.open('GET', 'http://example.com', true);
xhr.send(null);
/*
使用 POST 方式
xhr.open('POST', 'create.php', true);
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
xhr.send('name=王二狗&sex=男');
*/
```

## 使用 jQuery 的 AJAX
> jQuery.ajax(options)
### 选项属性说明
| 属性 | 说明 |
|-------|----------------------------------------------|
| type | POST 或 GET |
| url | 发送请求的地址 |
| data | 一个对象，连同请求发送到服务器的数据 |
| dataType | 预期服务器返回的数据类型。如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断，一般我们采用 json 格式，可以设置为 json |
| success | 一个方法，请求成功后的回调函数。传入返回的数据，以及包含成功代码的字符串 |
| error | 一个方法，请求失败时调用。传入 XMLHttpRequest 对象 |


## 使用fetch的AJAX
### 提交表单
```js
fetch('/submit', {
    method: 'post',
    body: new FormData(document.getElementById('comment-form'))
});
```

### 提交JSON
```js
fetch('/submit-json', {
    method: 'post',
    body: JSON.stringify({
        email: document.getElementById('email').value
        answer: document.getElementById('answer').value
    })
});
```

### 处理JSON响应
```js
fetch('https://davidwalsh.name/demo/arsenal.json').then(function(response) { 
    // 转换为 JSON
    return response.json();
}).then(function(json) {
    // 现在, json是一个 JavaScript object
    console.log(j); 
}).catch(function(err) {
	// 中途任何地方出错都在此处理
});
```

### 处理Text/HTML响应
```js
fetch('/next/page')
  .then(function(response) {
    return response.text();
  }).then(function(text) { 
    // <!DOCTYPE ....
    console.log(text); 
  });
```