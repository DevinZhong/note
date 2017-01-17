---
title: Bootstrap
copyright: true
date: 2016-09-28 15:47:14
tags: Boostrap
---

## 各种线条文本
```html
<!-- 删除的文本 -->
<del>Bootstrap</del>

<!-- 无用的文本 -->
<s>Bootstrap</s>

<!-- 插入的文本 -->
<ins>Bootstrap</ins>

<!-- 下划线文本 -->
<u>Bootstrap</u>
```

## 下拉菜单核心
1. 外围容器使用```class="dropdown"```包裹；
2. 内部点击按钮事件绑定```data-toggle="dropdown"```；
3. 菜单元素使用 ```class="dropdown-menu"```
4. 如果按钮再容器外部，可以通过```data-target```进行绑定

## 栅格系统
通过为“列（column）”设置 padding 属性，从而创建列与列之间的间隔（gutter）。通过为 .row 元素设置负值 margin 从而抵消掉为 .container 元素设置的 padding，也就间接为“行（row）”所包含的“列（column）”抵消掉了padding。

```html
<div>
  <div>.col-xs-6 .col-sm-3</div>
  <div>.col-xs-6 .col-sm-3</div>

  
  <div></div>

  <div>.col-xs-6 .col-sm-3</div>
  <div>.col-xs-6 .col-sm-3</div>
</div>
```

```html
<div>
  <div>.col-sm-6 .col-md-5 .col-lg-6</div>
  <div>.col-sm-6 .col-md-5 .col-md-offset-2 .col-lg-6 .col-lg-offset-0</div>
</div>
```

## 活用标签
<b> 用于高亮单词或短语，不带有任何着重的意味；而 <i> 标签主要用于发言、技术词汇等。


```html
You can use the mark tag to <mark>highlight</mark> text.
<del>This line of text is meant to be treated as deleted text.</del>
<ins>This line of text is meant to be treated as an addition to the document.</ins>
```

### 缩略词
```html
<abbr title="attribute">attr</abbr>
```

### 地址
```html
<address>
  <strong>Twitter, Inc.</strong><br>
  795 Folsom Ave, Suite 600<br>
  San Francisco, CA 94107<br>
  <abbr title="Phone">P:</abbr> (123) 456-7890
</address>

<address>
  <strong>Full Name</strong><br>
  <a href="mailto:#">first.last@example.com</a>
</address>

```

---
响应式表格
```html
<div>
  <table>
    ...
  </table>
</div>
```

```html
<div>
    <label>
      <input type="checkbox"> Check me out
    </label>
  </div>
```

不要将表单组直接和输入框组混合使用。建议将输入框组嵌套到表单组中使用。
一定要添加 label 标签（class='sr-only'隐藏）


---
```html
<form class="form-inline">
  <div>
    <label class="sr-only" for="exampleInputAmount">Amount (in dollars)</label>
    <div>
      <div>$</div>
      <input type="text" class="form-control" id="exampleInputAmount" placeholder="Amount">
      <div>.00</div>
    </div>
  </div>
  <button type="submit" class="btn btn-primary">Transfer cash</button>
</form>
```

---
水平排列表单，```.form-group```相当于```.row```
```html
<form class="form-horizontal">
  <div>
    <label for="inputEmail3" class="col-sm-2 control-label">Email</label>
    <div>
      <input type="email" class="form-control" id="inputEmail3" placeholder="Email">
    </div>
  </div>
  <div>
    <div>
      <div>
        <label>
          <input type="checkbox"> Remember me
        </label>
      </div>
    </div>
  </div>
```

---
禁用单选
```html
<div>
  <label>
    <input type="radio" name="optionsRadios" id="optionsRadios3" value="option3" disabled>
    Option three is disabled
  </label>
</div>
```

---
静态控件
```html
<form class="form-horizontal">
  <div>
    <label class="col-sm-2 control-label">Email</label>
    <div>
      <p>email@example.com</p>
    </div>
  </div>
```

---
只读状态
```html
<input class="form-control" type="text" placeholder="Readonly input here…" readonly>
```

---
```html
<button type="button" class="btn btn-link">（链接）Link</button>
```

如果 <a> 元素被作为按钮使用 -- 并用于在当前页面触发某些功能 -- 而不是用于链接其他页面或链接当前页面中的其他部分，那么，务必为其设置 role="button" 属性。
通过为按钮的背景设置 opacity 属性就可以呈现出无法点击的效果。

通过为图片添加 .img-responsive 类可以让图片支持响应式布局。其实质是为图片设置了 max-width: 100%;、 height: auto; 和 display: block; 属性，从而让图片在其父元素中更好的缩放。
如果需要让使用了 .img-responsive 类的图片水平居中，请使用 .center-block 类，不要用 .text-center。

---
图片形状
```html
<img src alt="...">
<img src alt="...">
<img src alt="...">
```

---
拉伸至父元素100%的宽度
```html
<button type="button" class="btn btn-primary btn-lg btn-block">（块级元素）Block level button</button>
```


```html
关闭按钮
<button type="button" class="close" aria-label="Close"><span>×</span></button>
三角符号
<span></span>
```

## 颜色
@brand-primary: darken(#428bca, 6.5%); // #337ab7
@brand-success: #5cb85c;
@brand-info:    #5bc0de;
@brand-warning: #f0ad4e;
@brand-danger:  #d9534f;