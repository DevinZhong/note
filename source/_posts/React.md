---
title: React
copyright: true
date: 2016-09-28 15:50:33
tags: React.js
---

## JSX
- `class` -> `className`
- `for` -> `htmlFor`


组件的数据来源，通常是通过 Ajax 请求从服务器获取，可以使用 componentDidMount 方法设置 Ajax 请求，等到请求成功，再用 this.setState 方法重新渲染 UI 


this.props.children属性表示组件的所有子节点：
- 没有子节点，undefined
- 一个子节点，object
- 多个子节点，array
```js
React.Children.map(this.props.children, function(child){
	return <li>{child}</li>;
})
```

## state不要存储重复的数据
## 设计state前的思考
1. 是否是从父级通过 props 传入的？如果是，可能不是 state 。
2. 是否会随着时间改变？如果不是，可能不是 state 。
3. 能根据组件中其它 state 数据或者 props 计算出来吗？如果是，就不是 state 。

直接改`state`并不会触发更新UI的操作，需要手动强制更新`forceUpdate()`。调用`setState()`方法才是正确的姿势。

---
子组件中
```javascript
getDefaultProps: function(){
    return{
        messages: ['默认的子消息']
    };
}
```


验证props：
```js
propTypes:{title: React.PropTypes.string.isRequired}
```

默认props：
```js
getDefaultProps: function(){
	return {title: 'Hello World'};
}
```


获取真实的DOM节点
- 元素上使用ref属性
```js
<input type="text" ref="myTextInput" />
```
- 必须等到虚拟DOM插入文档以后，才能使用这个属性this.refs.myTextInput，否则报错
```js
this.refs.myTextInput.focus();
```


## 初始化state
```js
getInitialState: function(){
	return {liked: false};
}
```


## 设置state
```js
this.setState({liked: !this.state.liked});
```


## 样式
~~style="opacity:{this.state.opacity}"~~
因为 React 组件样式是一个对象，所以第一重大括号表示这是 JavaScript 语法，第二重大括号表示样式对象。
```
style={{opacity: this.state.opacity}}
```