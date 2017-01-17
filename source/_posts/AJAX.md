---
title: AJAX
copyright: true
date: 2016-10-02 01:27:15
tags:
  - AJAX
  - JavaScript
---


## 使用XMLHttpRequeste的经典AJAX
```js
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function() {
    if (xhr.readyState == XMLHttpRequest.DONE) {
        alert(xhr.responseText);
    }
}
xhr.open('GET', 'http://example.com', true);
xhr.send(null);
```


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