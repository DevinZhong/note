---
title: JS常用技能
copyright: true
date: 2016-10-02 01:38:20
tags: JavaScript
---

## 删除数组元素
```js
photo = photos.splice(n, 1)[0];// 返回被删除的元素
```

## 数组切分
```js
var photos_left = photo.splice(0, Math.ceil(photos.length/2));
var photos_right = photos;
```

## 修改样式属性
```js
photo.style['-webkit-transform'] = `rotate(${random([-150, 150])}deg)`;
```

## 切换class样式
```js
function turn(elem){
	var cls = elem.className;
    if(/photo_front/.test(cls)){
    	cls = cls.replace(/photo_front/, 'photo_back')
    }else{
    	cls = cls.replace(/photo_back/, 'photo_front')
    }
    return elem.className = cls;
}
```

## 去除元素样式
```js
photo.className = photo.className.replace(/\s*photo_center\s*/, ' ');
```

## 为元素添加样式
```js
photo.className += ' photo_center ';
```

## 简易元素选择器
```js
funtion g(selector){
	var method = selector.charAt(0) == '.' ?
    	'getElementsByClassName' :
        'getElementById';
    return document[method](selector.substr(1));
}
```

## 模板内容替换
```js
var _html = template.replace('{{index}}', s).replace('{{img}}', data[s].img);
html.push(_html);
g('#wrap').innerHTML = html.join('');//最后联合
```

## 优化switch
```js
actionList = {
    a() {
        // do something
    },
    b() {
        // do something
    },
}
```