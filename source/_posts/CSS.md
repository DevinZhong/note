---
title: CSS
copyright: true
date: 2016-09-27 22:25:03
tags: CSS
---

> 这里记录 CSS 的点点滴滴
<!-- more -->

## 坐标
- x轴从左往右，y轴从上往下，原点于左上角

## 换行
- 默认情况下，长单词会另起一行放置，单词过长也不进行断词，保持溢出状态
- `word-wrap: break-word;` 会首先起一个新行来放置长单词，新的行放不下这个长单词则会对长单词进行断词
- `word-break: break-all;` 不会把长单词放在一个新行里，当这一行放不下的时候就直接断词

## z-index
- 仅在定位元素上生效
- 定位方式包括：`absolute`、`relative`、`fixed`、`inherit`（取决于父元素的定位方式）
- 默认的 `static` 定位将忽略 `z-index`

## CSS 权值
### 权值分为4个等级
1. 内联样式表的权值最高 1000；如：`style=""`
2. ID 选择器的权值为 100 ；如：`#content`
3. Class 类选择器的权值为 10 ；如：`.content`
4. HTML 标签选择器的权值为 1 ； 如：`div p`

### CSS优先级法则
1. 选择器都有一个权值，权值越大越优先；
2. 当权值相等时，后出现的样式表设置要优于先出现的样式表设置；
3. 浏览器默认的样式 < 网页制作者样式 < 用户自己设置的样式；
4. 继承的 CSS 样式不如后来指定的 CSS 样式；
5. 在同一组属性设置中标有 `!important` 规则的优先级最大；

## float
- 使用 `float` 脱离文档流时，其他盒子会无视这个元素，但其他盒子内的文本依然会为这个元素让出位置，环绕在其周围。
- 设置为 `float` 后，如果块没有指明自身大小，将根据自身内容进行自适应，然后才依据大小进行浮动

## 属性选择器
| 选择器 | 说明 |
|------------|-------------------------------------|
| [attribute~=value] | 用于选取属性值中包含指定词汇的元素 |
| [attribute|=value] | 用于选取带有以指定值开头的属性值的元素，该值必须是整个单词 |
| [attribute^=value] | 匹配属性值以指定值开头的每个元素 |
| [attribute$=value] | 匹配属性值以指定值结尾的每个元素 |
| [attribute*=value] | 匹配属性值中包含指定值的每个元素 |

## 属性选择器
> 巧用属性选择器可以减少类的设置
```css
[draggable=true] {
    cursor: move;
}
```

## 常用 CSS3 样式
```css
* {
  /*透明度*/
  opacity: 0; /*看不见*/
  opacity: 1;
  /*padding和内容都会在border之内*/
  box-sizing: border-box;
  /*圆角*/
  border-radius: 1px 0 3px 4px;
  /*阴影，参数：h-shadow v-shadow blur(模糊距离)*/
  box-shadow: 10px 5px 5px black;
  box-shadow: 0 0 1px rgba(0, 0, 0, .01);
  /*过渡，参数：过渡属性 持续时间 效果函数 过渡延时*/
  transition: background-color 2s ease 1s;
  /*字体平滑*/
  -webkit-font-smoothing: antialiased;
  /*调整大小1为原大小*/
  --webkit-transform:scale(.48);


  /*当元素不面向屏幕时隐藏*/
  -webkit-backface-visibility: hidden;
  /*支持子元素的3D效果*/
  -webkit-transform-style: preserve-3d;
  /*定位位移以及沿着Y轴旋转*/
  -webkit-transform: translate(0px, 0px) rotateY(0deg);
  /*设置本身元素某个特效的动画过渡*/
  -webkit-transition: -webkit-transform 0.6s ease-in-out;
  /*设置3D元素距本身视图的距离 如果设为0表示不支持透视*/
  -webkit-perspective: 800px;
}
```


## 常用解决方案
### 防止文字被选中
```css
span {
  user-select: none;
}
```

### 改变滚动条样式
```css
/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/
::-webkit-scrollbar {
  width: 16px;
  height: 16px;
  background-color: #F5F5F5;
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
  border-radius: 10px;
  background-color: #F5F5F5;
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
  background-color: #555;
}
```

### 突出小三角
```sass
.container {
  position: relative;

  &::before {
    position: absolute;
    content: '';
    height: 0;
    width: 0;
    right: 100%;
    border: 10px solid transparent;
    border-right-color: #fafafa;
  }
}
```

### 文本可编辑
```css
span {
  contenteditable: true;
}
```

### 居中
- 传统方法设置垂直居中
```css
div {
  position: absolute;
  top: 50%;
  margin-top: -300; /*设置为高度一半*/
}
```
- CSS3 居中（水平）
```css
div {
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
}
```

### 换行相关
#### 不换行
```css
div {
  white-space: nowrap;
}
```

#### 长单词溢出换行
```css
div {
  word-wrap: break-word;
}
```

#### 强行断词换行
```css
div {
  word-break: break-all;
}
```

### 设置大小可调整
```css
.resizable {
  overflow: scroll;
  resize: both;
  max-width: 300px;
  max-height: 460px;
}
```

### 弹出模态框后禁止屏幕滚动
有两种解决方案：
1. 使用 overflow
```js
function showDialog() {
  document.body.style.overflow = 'fixed'
  document.documentElement.style.position = 'fixed' // 未测试是否必要
}
function hideDialog() {
  document.body.style.overflow = 'auto'
  document.documentElement.style.position = 'auto' // 未测试是否必要
}
```

2. 使用 position
```js
let scrollTop
function showDialog() {
  scrollTop = document.body.scrollTop
  document.body.style.position = 'fixed'
  document.body.style.top = `-${scrollTop}px`
}
function hideDialog() {
  document.body.style.position = ''
  document.body.scrollTop = scrollTop
}
```