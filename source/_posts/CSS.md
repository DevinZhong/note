---
title: CSS
copyright: true
date: 2016-09-27 22:25:03
tags: CSS
---

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

### 传统方法设置垂直居中
```css
div {
  position: absolute;
  top: 50%;
  margin-top: -300; /*设置为高度一半*/
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